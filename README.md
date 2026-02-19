# Platform Architecture & Roadmap

**Status**: Draft  
**Last Updated**: Feb 2026  
**Owner**: Engineering

## What We're Building

A multi-tenant SaaS platform with AI capabilities. Think Notion meets Zapier with a layer of AI on top. Users get workspaces, can install apps from a marketplace, and automate workflows.

## Core Decisions

### Stack
- **Backend**: FastAPI + PostgreSQL + Redis
- **Frontend**: Next.js 14 (App Router) + TypeScript
- **AI**: OpenAI API + Pinecone for vector search
- **Infra**: Docker + AWS (ECS Fargate for prod, EC2 for staging)
- **Monitoring**: Sentry + Prometheus + Grafana (self-hosted)

### Why This Stack?

**FastAPI over Django/Node**: We need async for AI calls and real-time features. FastAPI gives us that without the Django baggage. Type safety with Pydantic is non-negotiable for an API-heavy product.

**Next.js 14 App Router**: Server Components let us move logic to the edge. Reduces client JS bundle significantly. RSC is the future, might as well start there.

**PostgreSQL**: We need ACID for transactions, JSONB for flexibility. Don't need Mongo's complexity yet.

**Redis**: Caching + Celery broker + rate limiting. One less service to manage.

**Self-hosted monitoring**: Sentry Cloud gets expensive fast. Prometheus + Grafana on ECS costs ~$150/month vs $500+ for managed.

## Architecture

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   CloudFront    │────▶│   ALB           │────▶│   ECS Fargate   │
│   (CDN/Static)  │     │   (Load Bal)    │     │   (Next.js)     │
└─────────────────┘     └─────────────────┘     └─────────────────┘
                                                         │
                                                         ▼
                                               ┌─────────────────┐
                                               │   ECS Fargate   │
                                               │   (FastAPI)     │
                                               └─────────────────┘
                                                         │
                    ┌────────────────────────────────────┼────────────────────────────────────┐
                    │                                    │                                    │
                    ▼                                    ▼                                    ▼
           ┌─────────────────┐                 ┌─────────────────┐                 ┌─────────────────┐
           │   RDS           │                 │   ElastiCache   │                 │   Pinecone      │
           │   PostgreSQL    │                 │   Redis         │                 │   (Vectors)     │
           └─────────────────┘                 └─────────────────┘                 └─────────────────┘
```

## 12-Month Roadmap

### Phase 1: Foundation (Months 1-3)
**Goal**: Internal alpha, core team can dogfood

**Backend**:
- Multi-tenant auth (JWT + workspace isolation)
- Basic CRUD APIs for 3 core entities
- Alembic migrations pipeline
- Sentry integration
- Unit test framework (pytest)

**Frontend**:
- Auth flows (login/signup)
- Dashboard shell
- 3 core feature UIs
- Error boundaries + Sentry

**DevOps**:
- Staging environment
- CI/CD (GitHub Actions)
- Docker Compose for local dev

**Team**: 2 backend, 2 frontend, 1 devops

### Phase 2: AI & Automation (Months 4-6)
**Goal**: Public beta, early adopters

**Backend**:
- OpenAI integration with streaming
- Vector search (Pinecone)
- Celery for background jobs
- Webhook system
- Rate limiting

**Frontend**:
- AI chat interface
- Real-time updates (WebSockets)
- Workflow builder UI
- Mobile responsiveness pass

**DevOps**:
- Production environment
- Monitoring stack (Prometheus/Grafana)
- Automated backups
- Log aggregation

**Team**: +1 ML engineer, +1 designer

### Phase 3: Scale & Polish (Months 7-9)
**Goal**: GA launch, first paying customers

**Backend**:
- Performance optimization
- Read replicas
- Advanced caching
- API versioning
- Compliance (GDPR/SOC2 prep)

**Frontend**:
- Advanced features
- Performance pass (Lighthouse 90+)
- E2E tests (Playwright)
- PWA features

**DevOps**:
- Auto-scaling policies
- Blue/green deployments
- Disaster recovery runbooks
- Cost optimization

**Team**: +1 backend, +1 frontend, +1 QA

### Phase 4: Enterprise (Months 10-12)
**Goal**: Enterprise features, $100k ARR

**Backend**:
- SAML/SSO
- Audit logs
- Advanced permissions
- API rate limits per tenant
- Custom integrations

**Frontend**:
- Admin panels
- Analytics dashboards
- White-labeling

**DevOps**:
- Multi-region deployment
- SOC 2 certification
- Penetration testing

## Key Technical Risks

1. **AI Latency**: Streaming responses are table stakes. Build for it from day 1.
2. **Multi-tenancy**: Get the data isolation model right early. Hard to change later.
3. **WebSocket Scale**: Don't use them for everything. SSE or polling for most, WS only for real-time collaboration.
4. **Vector DB Costs**: Pinecone gets expensive. Have a strategy for data retention and cleanup.

## Hiring Plan

**Month 1**: Core team (5 engineers)
- Senior Backend (FastAI/PostgreSQL expert)
- Mid Backend (APIs, async)
- Senior Frontend (Next.js 14)
- Mid Frontend (React/TypeScript)
- DevOps (AWS, Docker, CI/CD)

**Month 4**: AI & Growth (3 engineers)
- ML Engineer (LLMs, embeddings)
- Mid Backend (Celery, webhooks)
- Designer (UI/UX)

**Month 7**: Scale (3 engineers)
- Senior Backend (performance, scale)
- QA Engineer (automation)
- Mid Frontend (features)

## Budget Estimates

### People (Annual)
- 5 core engineers: $750k-900k
- 3 growth engineers: $450k-540k  
- 3 scale engineers: $450k-540k
- **Total**: $1.65M-1.98M

### Infrastructure (Monthly)

**Staging** ($200-400/month):
- EC2 t3.medium (Next.js): $30
- EC2 t3.medium (FastAPI): $30
- RDS db.t3.micro: $15
- ElastiCache cache.t3.micro: $15
- ALB: $20
- Data transfer: $50
- S3: $10
- Misc: $30

**Production** ($1,500-3,000/month at launch):
- ECS Fargate (2 services, 2 tasks each): $400
- RDS db.r6g.large: $350
- ElastiCache cache.r6g.large: $200
- ALB: $25
- CloudFront: $100
- S3: $50
- OpenAI API: $300-800
- Pinecone: $70-200
- Data transfer: $200
- Monitoring: $150

**At 10k MAU**: ~$8,000-12,000/month

## Decision Log

| Date | Decision | Context |
|------|----------|---------|
| Feb 2026 | FastAPI over Django | Async support, lighter weight, team's Python expertise |
| Feb 2026 | Next.js 14 App Router | Server Components reduce bundle size, better for SEO |
| Feb 2026 | Self-hosted monitoring | Cost control, Sentry Cloud pricing doesn't scale |
| Feb 2026 | PostgreSQL over Mongo | ACID requirements, JSONB gives us flexibility |
| Feb 2026 | Pinecone over Chroma | Managed service, scales without ops overhead |

## Open Questions

1. Do we need a mobile app (React Native) or is PWA sufficient for v1?
2. Should we support on-premise deployments for enterprise?
3. What's our data retention policy for AI conversations?
4. Compliance requirements: SOC2 Type II timeline?

## Next Steps

1. [ ] Finalize technical architecture document
2. [ ] Set up AWS accounts and billing alerts
3. [ ] Create job postings for Month 1 hires
4. [ ] Draft API design for core entities
5. [ ] Set up staging environment skeleton

---

Questions? Drop them in #engineering-architecture
