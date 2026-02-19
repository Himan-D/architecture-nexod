# Infrastructure

**Owner**: DevOps + Himanshu

## Two Environments

### Dev: Supabase
- PostgreSQL 15
- Built-in auth (only for dev)
- Real-time subscriptions (useful for agent events)
- Free tier

**When to use**: Local dev, testing, intern onboarding.

### Prod: AWS
- RDS PostgreSQL 15 (Multi-AZ)
- ECS Fargate (containers)
- S3 + CloudFront (assets)
- ElastiCache Redis
- Pinecone (vectors)

**When to use**: Production. Obviously.

## Agent Stack

### CrewAI
Multi-agent orchestration.
- Research crew: Researcher → Analyst → Writer
- Review crew: Auditor → Validator

State stored in PostgreSQL via crewai-tools.

### LangGraph
State machines with human-in-the-loop.
- Complex approval workflows
- Conditional branching
- Checkpointing in PostgreSQL, Redis for fast recovery

### Pinecone
Vector store for RAG.
- Document embeddings
- Agent memory
- Semantic search

## Hosting

### Frontend: Vercel
- and staging
- Preview URLs for every PR
- Zero-config. Git push Production, it deploys.

### Backend: AWS ECS Fargate
- Containers, no servers to manage
- Auto-scaling
- Why not serverless functions? Agent tasks run long. 30s+ timeouts kill us.

## Database

### Production (RDS)
- db.r6g.large to start
- Multi-AZ
- Automated backups
- 7-day retention

### Dev (Supabase)
- Free tier is fine
- Instant setup for new people

## Auth: Clerk

Frontend:
```javascript
import { ClerkProvider } from '@clerk/nextjs'
```

Backend:
```python
from clerk_backend_api import Clerk
clerk = Clerk(bearer_auth=settings.CLERK_SECRET_KEY)
```

OAuth, MFA, webhooks. Best DX.

## Monitoring

### Sentry
Errors and performance traces.
```python
sentry_sdk.init(
    dsn=settings.SENTRY_DSN,
    traces_sample_rate=0.1,
)
```

### Py-spy
When something's slow.
```bash
py-spy top --pid <pid>
py-spy record -o profile.svg --pid <pid>
```

### Prometheus + Grafana
Custom metrics. Agent execution time, API latency.

## Caching

### Redis Layers

**Sessions**:
```python
redis.setex(f"session:{user_id}", 3600, session_data)
```

**Agent State**:
```python
redis.setex(f"agent:{agent_id}:state", 300, state_json)
```

**API Responses**:
```python
@cache_for(seconds=60)
def get_agent_results(agent_id):
    return db.query(...)
```

## Latency Targets

| Metric | Target | Alert |
|--------|--------|-------|
| API p95 | <300ms | >500ms |
| Agent step | <5s | >10s |
| DB | <50ms | >100ms |
| Vector | <100ms | >200ms |

## CI/CD

GitHub Actions. Test → Staging → Production.

```yaml
test:
  - pytest
  - npm test

deploy-staging:
  if: branch = develop

deploy-production:
  if: branch = main
```

---

himanshu.dixit@nexod.ai
