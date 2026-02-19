# Monitoring & Observability Setup

**Status**: Production Ready  
**Owner**: Platform Team  
**Last Updated**: Feb 2026

## Philosophy

We monitor three things:
1. **Errors** - Is it broken? (Sentry)
2. **Performance** - Is it slow? (Prometheus + custom metrics)
3. **Business** - Are users converting? (Mixpanel/Amplitude)

Everything else is noise.

## Sentry Setup

**Why Sentry**: Best-in-class error tracking. Don't roll your own.

### FastAPI Integration

```python
# app/core/sentry.py
import sentry_sdk
from sentry_sdk.integrations.fastapi import FastApiIntegration
from sentry_sdk.integrations.sqlalchemy import SqlalchemyIntegration

def init_sentry():
    if settings.SENTRY_DSN:
        sentry_sdk.init(
            dsn=settings.SENTRY_DSN,
            environment=settings.ENVIRONMENT,
            release=settings.GIT_SHA,
            traces_sample_rate=0.1,  # 10% of requests for performance
            profiles_sample_rate=0.01,  # 1% for profiling
            integrations=[
                FastApiIntegration(),
                SqlalchemyIntegration(),
            ],
            before_send=filter_sensitive_data,
        )

def filter_sensitive_data(event, hint):
    # Remove PII before sending to Sentry
    if event.get("request", {}).get("data"):
        # Filter out passwords, tokens, etc.
        pass
    return event
```

```python
# app/main.py
from app.core.sentry import init_sentry

init_sentry()
app = FastAPI()
```

### Next.js Integration

```typescript
// sentry.client.config.ts
import * as Sentry from '@sentry/nextjs';

Sentry.init({
  dsn: process.env.NEXT_PUBLIC_SENTRY_DSN,
  environment: process.env.NEXT_PUBLIC_ENVIRONMENT,
  tracesSampleRate: 0.1,
  replaysSessionSampleRate: 0.01,
  replaysOnErrorSampleRate: 1.0,
  integrations: [
    Sentry.replayIntegration({
      maskAllText: true,
      blockAllMedia: true,
    }),
  ],
});
```

```typescript
// sentry.server.config.ts
import * as Sentry from '@sentry/nextjs';

Sentry.init({
  dsn: process.env.SENTRY_DSN,
  environment: process.env.ENVIRONMENT,
  tracesSampleRate: 0.1,
});
```

### What to Monitor in Sentry

**Issues**:
- All 500 errors
- Unhandled exceptions
- Frontend crashes
- API timeouts

**Performance**:
- API response time (p95, p99)
- Database query time
- Frontend page load time
- LCP, FID, CLS (Core Web Vitals)

**Alerts**:
- > 10 errors in 5 min → Slack #alerts
- > 100 errors in 5 min → Page on-call
- New error type → Slack #engineering
- Performance regression > 20% → Slack #alerts

## Prometheus + Grafana

**Why Self-Hosted**: Sentry performance monitoring gets expensive ($0.00025/transaction). At 1M requests/day = $7,500/month. Prometheus costs $150/month on EC2.

### Setup

```yaml
# docker-compose.monitoring.yml
version: '3.8'
services:
  prometheus:
    image: prom/prometheus:latest
    volumes:
      - ./prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus-data:/prometheus
    ports:
      - "9090:9090"
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'
      - '--storage.tsdb.retention.time=30d'
  
  grafana:
    image: grafana/grafana:latest
    ports:
      - "3001:3000"
    volumes:
      - grafana-data:/var/lib/grafana
      - ./grafana/dashboards:/etc/grafana/provisioning/dashboards
      - ./grafana/datasources:/etc/grafana/provisioning/datasources
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=changeme
  
  node-exporter:
    image: prom/node-exporter:latest
    ports:
      - "9100:9100"
  
  cadvisor:
    image: gcr.io/cadvisor/cadvisor:latest
    ports:
      - "8080:8080"
    volumes:
      - /:/rootfs:ro
      - /var/run:/var/run:ro
      - /sys:/sys:ro
      - /var/lib/docker/:/var/lib/docker:ro

volumes:
  prometheus-data:
  grafana-data:
```

```yaml
# prometheus.yml
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'fastapi'
    static_configs:
      - targets: ['fastapi:8000']
    metrics_path: '/metrics'
  
  - job_name: 'nextjs'
    static_configs:
      - targets: ['nextjs:3000']
  
  - job_name: 'node-exporter'
    static_configs:
      - targets: ['node-exporter:9100']
  
  - job_name: 'cadvisor'
    static_configs:
      - targets: ['cadvisor:8080']
```

### FastAPI Metrics

```python
# app/core/metrics.py
from prometheus_client import Counter, Histogram, Gauge, generate_latest
from fastapi import Request, Response
import time

# Request metrics
http_requests_total = Counter(
    'http_requests_total',
    'Total HTTP requests',
    ['method', 'endpoint', 'status_code']
)

http_request_duration_seconds = Histogram(
    'http_request_duration_seconds',
    'HTTP request duration',
    ['method', 'endpoint'],
    buckets=[.005, .01, .025, .05, .075, .1, .25, .5, .75, 1.0, 2.5, 5.0, 7.5, 10.0]
)

db_connections_active = Gauge('db_connections_active', 'Active database connections')

# Middleware
@app.middleware("http")
async def metrics_middleware(request: Request, call_next):
    start_time = time.time()
    
    response = await call_next(request)
    
    duration = time.time() - start_time
    
    http_requests_total.labels(
        method=request.method,
        endpoint=request.url.path,
        status_code=response.status_code
    ).inc()
    
    http_request_duration_seconds.labels(
        method=request.method,
        endpoint=request.url.path
    ).observe(duration)
    
    return response

@app.get("/metrics")
async def metrics():
    return Response(content=generate_latest(), media_type="text/plain")
```

### Key Metrics to Track

**Application**:
- Request rate (RPS)
- Response time (p50, p95, p99)
- Error rate (4xx, 5xx)
- Active connections

**Database**:
- Query duration
- Active connections
- Slow queries (>100ms)
- Connection pool utilization

**Infrastructure**:
- CPU usage
- Memory usage
- Disk I/O
- Network I/O

**Business** (custom metrics):
- Signup conversion
- Feature adoption
- API usage by endpoint

## Logging

**Philosophy**: Structured JSON logs. One line per request. Queryable.

### FastAPI

```python
# app/core/logging.py
import structlog
import logging
from pythonjsonlogger import jsonlogger

def setup_logging():
    structlog.configure(
        processors=[
            structlog.stdlib.filter_by_level,
            structlog.stdlib.add_logger_name,
            structlog.stdlib.add_log_level,
            structlog.processors.TimeStamper(fmt="iso"),
            structlog.processors.StackInfoRenderer(),
            structlog.processors.format_exc_info,
            structlog.processors.JSONRenderer()
        ],
        context_class=dict,
        logger_factory=structlog.stdlib.LoggerFactory(),
        wrapper_class=structlog.stdlib.BoundLogger,
        cache_logger_on_first_use=True,
    )
    
    # Configure standard library logging
    log_handler = logging.StreamHandler()
    formatter = jsonlogger.JsonFormatter(
        '%(timestamp)s %(level)s %(name)s %(message)s'
    )
    log_handler.setFormatter(formatter)
    
    root_logger = logging.getLogger()
    root_logger.addHandler(log_handler)
    root_logger.setLevel(logging.INFO)

# Usage
logger = structlog.get_logger()
logger.info("user_signup", user_id="123", plan="pro")
# Output: {"timestamp": "2026-02-19T10:00:00", "level": "info", "name": "app", "event": "user_signup", "user_id": "123", "plan": "pro"}
```

### Correlation IDs

Pass a `request_id` through the entire request lifecycle:

```python
# Middleware to set correlation ID
import uuid
from contextvars import ContextVar

request_id_ctx = ContextVar('request_id', default=None)

@app.middleware("http")
async def correlation_id_middleware(request: Request, call_next):
    request_id = request.headers.get('X-Request-ID', str(uuid.uuid4()))
    request_id_ctx.set(request_id)
    
    response = await call_next(request)
    response.headers['X-Request-ID'] = request_id
    
    return response

# Bind to logger
@app.middleware("http")
async def logging_middleware(request: Request, call_next):
    logger = structlog.get_logger().bind(
        request_id=request_id_ctx.get(),
        method=request.method,
        path=request.url.path,
    )
    
    start_time = time.time()
    response = await call_next(request)
    duration = time.time() - start_time
    
    logger.info(
        "request_completed",
        status_code=response.status_code,
        duration_ms=round(duration * 1000, 2)
    )
    
    return response
```

## Health Checks

```python
# app/api/health.py
from fastapi import APIRouter, Depends, status
from sqlalchemy.ext.asyncio import AsyncSession
from sqlalchemy import text
import redis.asyncio as redis

router = APIRouter()

@router.get("/health", status_code=status.HTTP_200_OK)
async def health_check(db: AsyncSession = Depends(get_db)):
    checks = {}
    healthy = True
    
    # Database
    try:
        await db.execute(text("SELECT 1"))
        checks["database"] = "ok"
    except Exception as e:
        checks["database"] = f"error: {str(e)}"
        healthy = False
    
    # Redis
    try:
        r = redis.Redis.from_url(settings.REDIS_URL)
        await r.ping()
        checks["redis"] = "ok"
        await r.close()
    except Exception as e:
        checks["redis"] = f"error: {str(e)}"
        healthy = False
    
    if not healthy:
        raise HTTPException(
            status_code=status.HTTP_503_SERVICE_UNAVAILABLE,
            detail=checks
        )
    
    return {"status": "healthy", "checks": checks}

@router.get("/ready")
async def readiness_check():
    # App is initialized and ready for traffic
    return {"ready": True}
```

Use `/health` for load balancer health checks. Use `/ready` for Kubernetes readiness probes.

## Alerting Rules

```yaml
# alertmanager.yml
groups:
- name: application_alerts
  rules:
  - alert: HighErrorRate
    expr: rate(http_requests_total{status_code=~"5.."}[5m]) > 0.1
    for: 5m
    labels:
      severity: critical
    annotations:
      summary: "High error rate detected"
      description: "Error rate is {{ $value }} errors/sec"
  
  - alert: SlowAPI
    expr: histogram_quantile(0.95, rate(http_request_duration_seconds_bucket[5m])) > 2
    for: 5m
    labels:
      severity: warning
    annotations:
      summary: "API response time is slow"
      description: "95th percentile latency is {{ $value }}s"
  
  - alert: DatabaseConnectionsHigh
    expr: db_connections_active > 80
    for: 5m
    labels:
      severity: warning
    annotations:
      summary: "Database connection pool running low"
  
  - alert: ServiceDown
    expr: up == 0
    for: 1m
    labels:
      severity: critical
    annotations:
      summary: "Service {{ $labels.job }} is down"
```

## Runbooks

### High Error Rate

1. Check Sentry for new error types
2. Check recent deployments (did we just ship?)
3. Check database connection pool
4. Check external service status (OpenAI, Stripe, etc.)
5. If stuck: escalate to on-call

### Slow API

1. Check slow query log in RDS
2. Check Redis hit rate
3. Check for N+1 queries in Sentry traces
4. Scale up if CPU/memory high
5. Enable read replicas if needed

### Database Connections Exhausted

1. Check for connection leaks (not closing sessions)
2. Restart application (temporary fix)
3. Increase connection pool size
4. Check for long-running queries
5. Add connection timeout

## Costs

| Service | Monthly Cost | Notes |
|---------|-------------|-------|
| Sentry Cloud (10k errors) | $26 | Start here |
| Sentry Cloud (100k errors) | $80 | Scale up as needed |
| Prometheus + Grafana (self-hosted) | $150 | EC2 t3.medium |
| CloudWatch (AWS native) | $200+ | More expensive, skip initially |
| Datadog | $1000+ | Too expensive for now |

**Recommendation**: Start with Sentry Cloud for errors, self-host Prometheus for metrics. Move to self-hosted Sentry if error volume > 100k/month.

## On-Call Rotation

**Primary**: Responds within 15 min  
**Secondary**: Backup, responds within 30 min  
**Escalation**: Engineering manager, 1 hour

**PagerDuty Integration**:
- Critical alerts → Page immediately
- Warning alerts → Slack only during business hours

---

## Checklist

Before going to production:

- [ ] Sentry configured and tested
- [ ] Prometheus scraping metrics
- [ ] Grafana dashboards created
- [ ] Health checks implemented
- [ ] Structured logging working
- [ ] Correlation IDs passing through
- [ ] Alert rules configured
- [ ] On-call rotation set up
- [ ] Runbooks written
- [ ] Test alerts sent

---

Questions? #platform-team
