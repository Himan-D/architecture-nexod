# What We're Looking For

**Contact**: himanshu.dixit@nexod.ai

## Baseline Skills

### Everyone
- **Python**: async/await, type hints, dataclasses. Know the difference between sync and async code.
- **TypeScript**: strict mode, generics, utility types. Not "any".
- **Git**: rebase, squash, semantic commits. Clean history matters.
- **SQL**: JOINs, window functions, CTEs. Know how to read an EXPLAIN.
- **System design**: Trade-offs, not just "use X because it's popular".

### Backend
- **FastAPI**: Dependencies, middleware, background tasks. The whole picture.
- **SQLAlchemy 2.0**: Async ORM. Relationships, eager loading, when to use what.
- **Alembic**: Migrations. Forward, backward, data migrations. How to handle schema changes with data.
- **Agent frameworks**: CrewAI and LangGraph. Understand when to use which.
- **Testing**: pytest-asyncio, factories, integration tests. Test the thing, not the framework.

### Frontend
- **Next.js 14**: Server Components, streaming, parallel routes. The app router is different.
- **React**: Concurrent features, Suspense, error boundaries. Advanced patterns.
- **State**: React Query for server state, Zustand for client. URL state for sharing.
- **Performance**: Bundle analysis, Core Web Vitals. Don't guess, measure.
- **Testing**: React Testing Library, Playwright. User-centric testing.

### DevOps
- **Docker**: Multi-stage builds, layer caching, security scanning. Production-ready containers.
- **AWS**: ECS, RDS, ElastiCache. Networking. What lives where.
- **CI/CD**: GitHub Actions. Test, lint, deploy. Rollbacks when things break.
- **Monitoring**: Sentry, Prometheus, Grafana. Know what's broken before users tell you.
- **Database**: Replication, backups, query optimization. Don't learn on production.

## Agent Knowledge

This is the main thing we're building. You don't need to be an ML researcher but you need to understand:

- **LLMs**: How prompting works, temperature, tokens. What "上下文" means.
- **RAG**: Embeddings, vector search, chunking. Why you'd use this.
- **Agent patterns**: ReAct, plan-and-execute, reflection. How agents reason.
- **CrewAI**: Roles, tasks, processes, memory. Multi-agent orchestration.
- **LangGraph**: State machines, conditional edges, checkpoints. Complex flows.

**You should understand**:
- Multi-agent orchestration - when to use it, when not to
- State persistence - checkpointing, resume on failure
- Human-in-the-loop - approval workflows, interrupts
- Error handling - agents fail. Handle it.
- Rate limiting - one workflow shouldn't kill everything

## What We Ask

### System Design
Design an agent workflow for:
- Research and report generation
- Multi-step approval process
- Real-time collaboration

We're not looking for perfect answers. We're looking for how you think. Trade-offs. What's hard. What you'd do differently.

### Coding
- Build a FastAPI endpoint with agent integration
- Implement a React component with proper state
- Dockerize something with health checks

### Debugging
- Slow API endpoint. How do you find it?
- Agent timeout. What happened?
- Memory leak. Where's it coming from?
- Race condition. How do you prove it?

## How We Evaluate

1. **Problem solving** (30%) - How do you think? What questions do you ask?
2. **Code quality** (25%) - Clean, typed, tested.
3. **System design** (20%) - Trade-offs, scale considerations.
4. **Communication** (15%) - Can you explain it? Can you take feedback?
5. **Testing** (10%) - What would you test? How?

## Show Us

- GitHub - We want to see your code. Side projects count.
- Deployed thing - Even if it's small. Shows you can ship.
- Technical writing - Blog post, documentation, README. Shows you can explain.

---

himanshu.dixit@nexod.ai
