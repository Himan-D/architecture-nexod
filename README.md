# Nexod Platform Architecture

**Repository**: https://github.com/Himan-D/architecture-nexod  
**Owner**: Himanshu Dixit (himanshu.dixit@nexod.ai)  
**Last Updated**: Feb 2026

## What We Build

Multi-tenant SaaS platform with AI agents, workflow automation, and real-time collaboration. Core differentiator: autonomous agents that execute complex workflows.

## Quick Links

- [Core Architecture](./01-core/)
- [Team Structure](./02-team/)
- [Infrastructure](./03-infrastructure/)
- [Development](./04-development/)
- [Policies](./05-policies/)

## Tech Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| Frontend | Next.js 14, TypeScript, Tailwind | Web app, rapid prototyping |
| Backend | FastAPI, Python 3.11 | APIs, async processing |
| Agents | CrewAI, LangGraph | Autonomous workflows |
| Database | PostgreSQL (Prod), Supabase (Dev) | Primary data |
| Vector | Pinecone | Embeddings, RAG |
| Auth | Clerk | User management |
| Cache | Redis | Sessions, rate limiting |
| Hosting | Vercel (FE), AWS ECS (BE) | Scale, performance |
| Monitor | Sentry, Py-spy, Prometheus | Observability |

## Team Structure

**Layer 1 - Leadership (2)**
- Himanshu Dixit: Architecture, backend, agents
- Senior Frontend Lead: Frontend, design system, Figma MCP

**Layer 2 - Core Team (5)**
- 2 Backend Interns: APIs, database, agent integration
- 2 Frontend Interns: UI, components, rapid prototyping
- 1 DevOps Intern: Infrastructure, CI/CD, monitoring

## Communication Stack

| Tool | Purpose |
|------|---------|
| **Gmail** | Official, external comms |
| **Google Chat** | Daily chat, quick questions |
| **Google Drive** | All documentation, RFCs |
| **Trello** | Task management, sprints |
| **GitHub** | Code, issues, PRs |
| **Figma** | Design, rapid prototyping |
| **Loom** | Async video explanations |

## Database Strategy

**Production**: AWS RDS PostgreSQL
- Multi-AZ, automated backups, read replicas
- CrewAI state persistence
- LangGraph checkpointing

**Development**: Supabase
- Zero-config, free tier
- Real-time subscriptions for agent events
- Instant intern onboarding

## Agent Architecture

**CrewAI**: Multi-agent orchestration
- Research agents
- Execution agents  
- Review agents

**LangGraph**: State machines for complex flows
- Conditional edges
- Human-in-the-loop
- State persistence

**Vector Store**: Pinecone
- RAG for agents
- Long-term memory
- Semantic search

## Key Principles

1. **Ship weekly** - Deploy every Friday
2. **Measure everything** - No metrics = no feature
3. **Reuse components** - Build once, use everywhere
4. **Agent-first** - Design for autonomous workflows
5. **AI-assisted, human-approved** - All code reviewed

## Contact

- **Email**: himanshu.dixit@nexod.ai
- **Google Chat**: himanshu.dixit@nexod.ai
- **GitHub**: @Himan-D
