# Nexod Engineering

**Repository**: https://github.com/Himan-D/architecture-nexod  
**Lead**: Himanshu Dixit (himanshu.dixit@nexod.ai)

## What We're Building

Real estate platform like Propnet.ai. Uber for real estate - buyers, sellers, and agents all in one place.

AI agents handle the heavy lifting - property matching, scheduling, follow-ups, market analysis.

**The core**: Autonomous agents that understand real estate. They match buyers to properties, schedule showings, handle negotiations.

## The Stack

| Layer | Tech | Why |
|-------|------|-----|
| Frontend | Next.js 14 + TypeScript | Server Components = less JS. This is a tool, not a toy app. |
| Backend | FastAPI + Python 3.11 | Async for real-time. Pydantic for type safety. |
| Agents | CrewAI + LangGraph | Different problems need different tools. |
| Database | PostgreSQL | ACID matters. JSONB for flexibility. |
| Vectors | Pinecone | RAG for agent memory. |
| Cache | Redis | Sessions, Celery, rate limiting. |
| Auth | Clerk | Best DX. Works. |
| Hosting | Vercel (FE), AWS ECS (BE) | Zero-config frontend. Long-running agent tasks don't work on serverless functions. |

## Real Estate Platform Challenges

This isn't a blog. Here's what makes it hard:

- **Real-time property status** - Available → under contract → sold. Everyone sees changes instantly.
- **Search on steroids** - 15+ filters, geo queries, sorting. Thousands of properties.
- **Media heavy** - 20-50 photos per property. Lazy loading, CDNs.
- **Maps integration** - Property markers, clustering, neighborhoods.
- **Scheduling** - Showings, open houses, time zones.
- **AI matching** - "I want something modern but near work" → actual properties.
- **Lead management** - Capture, assign, follow-up. Sales workflows.

## How We Work

**Ship weekly**. Every Friday.

**Type safety**. No "any".

**Sentry first**. Can't debug what you don't see.

**80% test coverage**. No exceptions.

## Communication

- **Google Chat** - Daily
- **Google Drive** - Docs
- **Trello** - Tasks
- **GitHub** - Code
- **Figma** - Design

## Quick Links

- [Core Architecture](./01-core/)
- [Team Structure](./02-team/)
- [Infrastructure](./03-infrastructure/)
- [Development](./04-development/)
- [Policies](./05-policies/)

## Getting Started

1. Read [Core Architecture](./01-core/)
2. Set up dev environment: [Setup](./03-infrastructure/SETUP.md)
3. Review [Standards](./04-development/STANDARDS.md)

## Contact

himanshu.dixit@nexod.ai
