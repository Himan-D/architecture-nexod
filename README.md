# Nexod Engineering

**Repository**: https://github.com/Himan-D/architecture-nexod  
**Lead**: Himanshu Dixit (himanshu.dixit@nexod.ai)  
**Focus**: Agent-native platform engineering

## What We Build

Autonomous agent platform. Multi-tenant SaaS where AI agents execute complex workflows end-to-end.

**Core**: CrewAI orchestration + LangGraph state machines + Real-time collaboration

## Stack

| Layer | Tech | Purpose |
|-------|------|---------|
| Frontend | Next.js 14, TypeScript, Tailwind | App, rapid prototyping |
| Backend | FastAPI, Python 3.11 | APIs, async agents |
| Agents | CrewAI, LangGraph | Multi-agent workflows |
| Vector | Pinecone | RAG, memory |
| DB | PostgreSQL | Primary data |
| Cache | Redis | Sessions, state |
| Auth | Clerk | Identity |
| Host | Vercel (FE), AWS ECS (BE) | Scale |
| Monitor | Sentry, Py-spy | Observability |

## Engineering Principles

1. **Agent-first architecture** - Design for autonomous workflows
2. **Type safety everywhere** - Pydantic, TypeScript strict
3. **Observability by default** - Metrics, traces, logs
4. **Performance budgets** - <300ms API, <5s agent response
5. **Test coverage >80%** - No exceptions

## Documentation

- [Core Architecture](./01-core/) - System design, decisions
- [Team Structure](./02-team/) - Roles, responsibilities
- [Infrastructure](./03-infrastructure/) - DevOps, monitoring
- [Development](./04-development/) - Patterns, standards
- [Policies](./05-policies/) - Engineering guidelines

## Communication

- **Google Chat** - Engineering discussions
- **Google Drive** - Architecture docs, RFCs
- **Trello** - Sprint planning
- **GitHub** - Code, reviews
- **Figma** - Design system, MCP workflows

## Quick Start

1. Read [Core Architecture](./01-core/)
2. Set up environment: [Infrastructure/SETUP](./03-infrastructure/SETUP.md)
3. Review [Development Standards](./04-development/STANDARDS.md)
4. Join Google Chat #engineering

## Contact

himanshu.dixit@nexod.ai
