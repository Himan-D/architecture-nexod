# Infrastructure

**Owner**: DevOps Intern + Himanshu Dixit

## Environments

### Development: Supabase
```
Database: PostgreSQL 15
Auth: Built-in (dev only)
Real-time: Enabled (for agent events)
Storage: 500MB
Cost: $0
```

**Use**: Local dev, agent testing, intern onboarding

### Production: AWS
```
Database: RDS PostgreSQL 15 (Multi-AZ)
Compute: ECS Fargate
Storage: S3 + CloudFront
Cache: ElastiCache Redis
Vector: Pinecone
Cost: ~$1,650/month
```

## Agent Stack

### CrewAI
**Use**: Multi-agent workflows
- Research crew: Researcher → Analyst → Writer
- Review crew: Auditor → Validator

**State**: PostgreSQL via crewai-tools

### LangGraph
**Use**: State machines, human-in-the-loop
- Complex approval workflows
- Conditional branching
- State persistence in PostgreSQL

**Checkpoint**: Redis for fast state recovery

### Pinecone
**Use**: Vector store for RAG
- Document embeddings
- Agent memory
- Semantic search

```python
# Pinecone setup
from pinecone import Pinecone
pc = Pinecone(api_key=settings.PINECONE_API_KEY)
index = pc.Index("nexod-knowledge")
```

## Hosting

### Frontend: Vercel
```
Production: https://app.nexod.ai
Staging: https://staging.nexod.ai
Preview: Every PR
```

**Why**: Next.js optimization, edge network, automatic HTTPS

### Backend: AWS ECS Fargate
```
Production: 2-10 tasks
Staging: 1 task (t3.medium)
```

**Why**: Long-running agents (30s+), WebSockets, cost control

## Database

### Production (RDS)
```yaml
Instance: db.r6g.large
Storage: 100GB GP3
Multi-AZ: Yes
Backups: 7 days
Encryption: Yes
```

**Agent Tables**:
- `agent_sessions`: Active agent runs
- `agent_logs`: Execution logs
- `vector_metadata`: Pinecone metadata

### Development (Supabase)
**Why**: Instant setup, real-time for agent events, free tier

## Auth: Clerk
```javascript
// Frontend
import { ClerkProvider } from '@clerk/nextjs'

// Backend
from clerk_backend_api import Clerk
clerk = Clerk(bearer_auth=settings.CLERK_SECRET_KEY)
```

**Features**: OAuth, MFA, webhooks for agent permissions

## Monitoring

### Sentry
```python
sentry_sdk.init(
    dsn=settings.SENTRY_DSN,
    traces_sample_rate=0.1,
)
```

**Alerts**:
- >10 errors/5min → Google Chat
- Agent failures → Immediate alert
- Performance regression → Chat

### Py-spy
```bash
py-spy top --pid <pid>
py-spy record -o profile.svg --pid <pid>
```

**Use**: Agent performance, API latency >500ms

### Prometheus + Grafana
**Self-hosted**: $80/month  
**Metrics**: Agent execution time, API latency, DB queries

## Caching

### Redis Layers

**L1: Sessions**
```python
redis.setex(f"session:{user_id}", 3600, session_data)
```

**L2: Agent State**
```python
redis.setex(f"agent:{agent_id}:state", 300, state_json)
```

**L3: API Responses**
```python
@cache_for(seconds=60)
def get_agent_results(agent_id):
    return db.query(...)
```

## Session Management

**Backend**: Redis  
**Frontend**: Zustand + React Query  
**Agent State**: LangGraph checkpointing (PostgreSQL + Redis)

## Latency Targets

| Metric | Target | Alert |
|--------|--------|-------|
| API p95 | <300ms | >500ms |
| Agent response | <5s | >10s |
| Database | <50ms | >100ms |
| Vector search | <100ms | >200ms |

## CI/CD

```yaml
name: Deploy
on: [push]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: cd backend && pytest
      - run: cd frontend && npm test
  
  deploy-staging:
    needs: test
    if: github.ref == 'refs/heads/develop'
    run: aws ecs update-service --cluster staging --service api
  
  deploy-production:
    needs: test
    if: github.ref == 'refs/heads/main'
    run: aws ecs update-service --cluster production --service api
```

## Costs

| Service | Monthly |
|---------|---------|
| Supabase | $0 |
| Vercel Pro | $20 |
| AWS ECS (staging) | $150 |
| AWS ECS (prod) | $800 |
| RDS PostgreSQL | $350 |
| ElastiCache | $200 |
| Pinecone | $70 |
| S3 + CloudFront | $100 |
| Sentry | $26 |
| **Total** | **~$1,716** |

## Setup

See [SETUP.md](./SETUP.md)

---

Questions: himanshu.dixit@nexod.ai
