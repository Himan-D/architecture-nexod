# Monitoring

**What we watch**: Errors, performance, business metrics.

## Sentry

Error tracking and performance.

### Backend
```python
import sentry_sdk
from sentry_sdk.integrations.fastapi import FastApiIntegration

sentry_sdk.init(
    dsn=settings.SENTRY_DSN,
    traces_sample_rate=0.1,
    integrations=[FastApiIntegration()],
)
```

### Frontend
```typescript
import * as Sentry from '@sentry/nextjs';

Sentry.init({
  dsn: process.env.NEXT_PUBLIC_SENTRY_DSN,
  tracesSampleRate: 0.1,
});
```

### Alerts
- >10 errors/5min → Google Chat
- >100 errors/5min → Page
- Performance regression → Chat

## Py-spy

Python profiling. Zero overhead in production.

```bash
# See what's slow
py-spy top --pid <pid>

# Record flamegraph
py-spy record -o profile.svg --pid <pid>
```

Use when: API slow, memory high, CPU spikes.

## Prometheus + Grafana

Custom metrics. Self-hosted.

### Metrics
```python
from prometheus_client import Counter, Histogram

agent_runs = Counter('agent_runs_total', 'Agent runs', ['agent_type', 'status'])
api_duration = Histogram('api_duration_seconds', 'API latency', ['endpoint'])
```

### Dashboards
- API latency (p50, p95, p99)
- Agent execution time
- Error rates
- Database queries

## Health Checks

```python
@app.get("/health")
async def health(db: AsyncSession = Depends(get_db)):
    await db.execute(text("SELECT 1"))
    return {"status": "ok"}

@app.get("/ready")
async def ready():
    return {"ready": True}
```

Use `/health` for load balancer probes, `/ready` for Kubernetes.

---

himanshu.dixit@nexod.ai
