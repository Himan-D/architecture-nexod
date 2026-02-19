# Nexod Engineering

**Repository**: https://github.com/Himan-D/architecture-nexod  
**Lead**: Himanshu Dixit (himanshu.dixit@nexod.ai)

## What We're Building

Agent platform. Users create workflows, AI agents execute them autonomously. Think Zapier meets Claude - but the agents actually do the work, not just move data around.

**The core bet**: Autonomous agents that can handle multi-step workflows with memory, reasoning, and human-in-the-loop checkpoints.

## The Stack

| Layer | Tech | Why This One |
|-------|------|-------------|
| Frontend | Next.js 14 + TypeScript | Server Components = less JS shipping. We're not building a SPA, this is a tool. |
| Backend | FastAPI + Python 3.11 | Async is non-negotiable for agent execution. Pydantic for type safety at API boundaries. |
| Agents | CrewAI + LangGraph | CrewAI for multi-agent orchestration, LangGraph for complex state machines. Different tools for different jobs. |
| Database | PostgreSQL | Need ACID. JSONB for agent state. Don't reach for Mongo until we have a reason. |
| Vectors | Pinecone | Managed vector DB. Don't want to ops our own. RAG is core to agent memory. |
| Cache | Redis | Sessions, Celery broker, rate limiting. Standard stuff. |
| Auth | Clerk | Best DX. Pre-built components. Webhooks keep our DB in sync. |
| Hosting | Vercel (frontend), AWS ECS (backend) | Vercel for zero-config Next.js. ECS for long-running agent tasks that would timeout on serverless. |

## How We Work

**Ship weekly**. Every Friday. Doesn't have to be big, but we deploy.

**Type safety everywhere**. If it's not typed, it's not done. Pydantic, TypeScript strict, mypy.

**Observability first**. Can't debug what you can't see. Sentry for errors, custom metrics for agent performance.

**Test your code**. 80% coverage minimum. No exceptions for "simple" endpoints.

## Communication

- **Google Chat** - Day-to-day. Threads, not walls of text.
- **Google Drive** - Architecture docs, RFCs. Not in Notion, not in Confluence.
- **Trello** - Sprint tracking. Simple boards.
- **GitHub** - Code lives here. Issues, PRs, Actions.
- **Figma** - Design. We're using MCP for rapid prototyping.

## Why Not X?

We've made these choices. Here's the thinking:

- **FastAPI over Django**: We need async. Django's sync-first mindset fights us. FastAPI gives us asyncio-native everything.
- **PostgreSQL over Mongo**: We need transactions. Agent state, billing, user data - all relational. JSONB gives flexibility when we need it.
- **CrewAI + LangGraph**: CrewAI for "three agents collaborate on this". LangGraph for "this is a complex state machine with human checkpoints". Different tools.
- **Self-hosted PostgreSQL over Supabase in prod**: Control. Backups. Performance tuning. Supabase is great for dev.

## Quick Links

- [Core Architecture](./01-core/) - System design decisions
- [Team Structure](./02-team/) - Engineering roles
- [Infrastructure](./03-infrastructure/) - DevOps, monitoring
- [Development](./04-development/) - Coding standards
- [Policies](./05-policies/) - Engineering guidelines

## Getting Started

1. Read [Core Architecture](./01-core/)
2. Set up dev environment: [Infrastructure/SETUP](./03-infrastructure/SETUP.md)
3. Review [Development Standards](./04-development/STANDARDS.md)
4. Jump in #engineering on Google Chat

## Contact

himanshu.dixit@nexod.ai
