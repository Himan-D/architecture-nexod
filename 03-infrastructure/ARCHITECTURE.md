# Infrastructure & DevOps

**Owner**: DevOps Intern + Himanshu Dixit  
**Environments**: Development (Supabase), Production (AWS)

## Environments

### Development: Supabase
**Why**: Zero-config, instant setup, free tier

```
Database: PostgreSQL 15
Auth: Built-in (dev only)
Real-time: Enabled
Storage: 500MB
API: Auto-generated
```

**Use for**:
- Local development
- Feature testing
- Intern onboarding
- Demos

**Do NOT use for**:
- Production data
- Performance testing
- Security testing

### Production: AWS
**Why**: SLAs, compliance, scale

```
Database: RDS PostgreSQL 15 (Multi-AZ)
Compute: ECS Fargate
Storage: S3 + CloudFront
Cache: ElastiCache Redis
Monitoring: CloudWatch + Sentry
```

## Hosting Strategy

### Frontend: Vercel
**Why**: Zero-config deployments, edge network, Next.js optimization

```
Production: https://app.nexod.ai
Staging: https://staging.nexod.ai
Preview: Every PR gets URL
```

**Features**:
- Automatic HTTPS
- Global CDN
- Edge functions
- Analytics built-in
- Git integration

### Backend: AWS ECS Fargate
**Why**: Serverless containers, auto-scaling, pay-per-use

```
Production: ALB → ECS Fargate (2 tasks min, 10 max)
Staging: Single task, t3.medium
```

**Why not Vercel for backend?**:
- Vercel has 10s timeout on functions
- We need long-running AI calls (30s+)
- Need persistent connections (WebSockets)
- Better cost control at scale

## Database Architecture

### Production (RDS)
```yaml
Instance: db.r6g.large
Storage: 100GB GP3 (auto-scaling)
Multi-AZ: Yes
Backups: 7 days
Encryption: Yes
Public access: No
```

**Connection Pool**:
- Max: 100 connections
- Timeout: 30s
- PgBouncer for pooling

**Read Replicas**:
- 2 replicas for read-heavy workloads
- Route analytics to replicas

### Development (Supabase)
```yaml
Plan: Free tier
Database: PostgreSQL 15
API: REST + GraphQL
Auth: Built-in
Storage: 500MB
```

**Migration Process**:
1. Develop locally with Supabase
2. Test migrations in staging (AWS)
3. Production deploy with Alembic
4. Verify with health checks

## Authentication: Clerk
**Why**: Best-in-class DX, pre-built components, scalable

```javascript
// Frontend
import { ClerkProvider, SignIn } from '@clerk/nextjs'

// Middleware
import { authMiddleware } from '@clerk/nextjs'
export default authMiddleware({
  publicRoutes: ['/']
})
```

**Features**:
- Email/password
- OAuth (Google, GitHub)
- MFA
- User management dashboard
- Webhooks for backend sync

**Backend Verification**:
```python
from clerk_backend_api import Clerk

clerk = Clerk(bearer_auth=settings.CLERK_SECRET_KEY)
user = clerk.users.get(user_id)
```

## Monitoring Stack

### Sentry (Errors + Performance)
**Why**: Industry standard, great DX

**Setup**:
```python
# Backend
sentry_sdk.init(
    dsn=settings.SENTRY_DSN,
    traces_sample_rate=0.1,
    profiles_sample_rate=0.01,
)
```

```javascript
// Frontend
Sentry.init({
  dsn: process.env.NEXT_PUBLIC_SENTRY_DSN,
  tracesSampleRate: 0.1,
})
```

**Alert Rules**:
- >10 errors/5min → Slack
- >100 errors/5min → Page
- New issue type → Slack
- Performance regression → Slack

### Py-spy (Python Profiling)
**Why**: Zero-overhead profiling, production safe

**Install**: `pip install py-spy`

**Usage**:
```bash
# Top-like view
py-spy top --pid <pid>

# Record flamegraph
py-spy record -o profile.svg --pid <pid>

# Live dump
py-spy dump --pid <pid>
```

**When to use**:
- API latency >500ms
- Memory leaks suspected
- High CPU usage
- Before optimization (always profile first)

**Metrics to watch**:
- GIL contention
- Database query time
- JSON serialization
- Function call frequency

### Prometheus + Grafana (Metrics)
**Why**: Free, flexible, industry standard

**Self-hosted on ECS**:
- Prometheus: $50/month (t3.small)
- Grafana: $30/month (t3.micro)
- Storage: 30-day retention

**Key Metrics**:
```python
# Request metrics
http_requests_total = Counter('http_requests_total', ['method', 'endpoint', 'status'])
http_request_duration = Histogram('http_request_duration_seconds', ['method', 'endpoint'])

# Business metrics
active_users = Gauge('active_users', 'Currently active users')
tasks_created = Counter('tasks_created_total', ['user_type'])
```

### Memray (Memory Profiling)
**Why**: Python memory leaks

```bash
pip install memray
python -m memray run app.py
python -m memray flamegraph memray-output.bin
```

## Caching Strategy

### Redis Layers

**L1 - Application Cache**:
```python
# User sessions
redis.setex(f"session:{user_id}", 3600, session_data)

# API responses
redis.setex(f"api:{cache_key}", 300, json.dumps(data))
```

**L2 - Database Query Cache**:
```python
# Expensive queries
@cache_for(seconds=60)
def get_dashboard_data(user_id):
    return db.query(...)
```

**L3 - CDN Cache (Vercel/CloudFront)**:
```javascript
// Static assets, API responses
Cache-Control: public, max-age=3600, stale-while-revalidate=86400
```

### Cache Invalidation

**Pattern**:
```python
# Write-through cache
async def update_user(user_id, data):
    # Update DB
    await db.execute("UPDATE users SET ...")
    # Invalidate cache
    await redis.delete(f"user:{user_id}")
```

## Session Management

### Backend Sessions (Redis)
```python
# Store session
session_id = generate_token()
redis.setex(f"session:{session_id}", 3600, json.dumps(user_data))

# Verify session
session_data = redis.get(f"session:{session_id}")
if not session_data:
    raise Unauthorized()
```

### Frontend State (Zustand)
```typescript
// Global state
const useStore = create<Store>()(
  persist(
    (set, get) => ({
      user: null,
      setUser: (user) => set({ user }),
    }),
    { name: 'app-storage' }
  )
)
```

### Server State (React Query)
```typescript
// API caching
const { data } = useQuery({
  queryKey: ['tasks'],
  queryFn: fetchTasks,
  staleTime: 5 * 60 * 1000, // 5 min
})
```

## Latency Targets

| Metric | Target | Alert At |
|--------|--------|----------|
| API p50 | <100ms | >200ms |
| API p95 | <300ms | >500ms |
| API p99 | <500ms | >1s |
| Database | <50ms | >100ms |
| Redis | <5ms | >10ms |
| Frontend TTFB | <200ms | >500ms |
| Frontend LCP | <2.5s | >4s |

## CI/CD Pipeline

**GitHub Actions**:

```yaml
name: Deploy
on: [push]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Test Backend
        run: |
          cd backend
          pytest
      - name: Test Frontend  
        run: |
          cd frontend
          npm test
  
  deploy-staging:
    needs: test
    if: github.ref == 'refs/heads/develop'
    steps:
      - name: Deploy to Staging
        run: |
          aws ecs update-service --cluster staging --service api
  
  deploy-production:
    needs: test
    if: github.ref == 'refs/heads/main'
    steps:
      - name: Deploy to Production
        run: |
          aws ecs update-service --cluster production --service api
```

## Cost Optimization

**Current Estimate**:
- Supabase (dev): $0
- Vercel Pro: $20/month
- AWS ECS (staging): $150/month
- AWS ECS (production): $800/month
- RDS PostgreSQL: $350/month
- ElastiCache: $200/month
- S3 + CloudFront: $100/month
- Sentry: $26/month
- **Total**: ~$1,650/month

**Savings**:
- Use Spot instances for non-critical workloads
- RDS Reserved Instances (1-year commit)
- S3 Intelligent Tiering
- CloudFront caching

---

*Questions? Slack #devops or email himanshu.dixit@nexod.ai*
