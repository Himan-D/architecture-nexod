# Core Architecture

**Owner**: Himanshu Dixit  
**Last Updated**: Feb 2026

## Overview

Multi-tenant SaaS with autonomous AI agents as the core differentiator.

## Stack

| Component | Technology | Why |
|-----------|-----------|-----|
| Frontend | Next.js 14, TypeScript | App Router, Server Components |
| Backend | FastAPI, Python 3.11 | Async, type safety |
| Agents | CrewAI, LangGraph | Multi-agent, state machines |
| Database | PostgreSQL | ACID, JSONB flexibility |
| Vector | Pinecone | Embeddings, RAG |
| Auth | Clerk | DX, pre-built components |
| Cache | Redis | Sessions, rate limiting |
| Hosting | Vercel (FE), AWS ECS (BE) | Scale, performance |
| Monitor | Sentry, Py-spy | Errors, profiling |

## Agent Architecture

### CrewAI
Multi-agent orchestration for workflows.

```python
# Research crew
researcher = Agent(role='Researcher', ...)
analyst = Agent(role='Analyst', ...)
writer = Agent(role='Writer', ...)

crew = Crew(
    agents=[researcher, analyst, writer],
    tasks=[research_task, analyze_task, write_task],
    process=Process.sequential
)
```

### LangGraph
State machines for complex flows.

```python
# Approval workflow
workflow = StateGraph(AgentState)
workflow.add_node("research", research_node)
workflow.add_node("approve", human_approval_node)
workflow.add_conditional_edges("research", needs_approval)

app = workflow.compile()
```

### Pinecone
Vector storage for RAG and memory.

```python
index = pc.Index("nexod-knowledge")
index.upsert(vectors=[(id, embedding, metadata)])
```

## System Architecture

```
User → CloudFront → Vercel (Next.js)
                    ↓
              ALB → ECS Fargate (FastAPI)
                    ↓
       ┌────────────┼────────────┐
       ↓            ↓            ↓
   RDS (PG)   ElastiCache   Pinecone
```

## Key Decisions

| Decision | Why |
|----------|-----|
| FastAPI over Django | Async for agents, lighter |
| Next.js 14 App Router | Server Components, less JS |
| CrewAI + LangGraph | Different use cases, both needed |
| Pinecone | Managed, scales well |
| Clerk | Best DX, scales |

## Roadmap

**Phase 1 (Weeks 1-4)**: Foundation
- Auth, basic APIs
- Agent hello-world
- Dev environment

**Phase 2 (Weeks 5-8)**: AI Core
- CrewAI integration
- LangGraph workflows
- Vector search

**Phase 3 (Weeks 9-12)**: Scale
- Performance optimization
- Advanced agents
- Production hardening

## Risks

1. **AI latency**: Build streaming from day 1
2. **Multi-tenancy**: Get isolation right early
3. **Agent failures**: Retry logic, human handoff
4. **Vector scale**: Have cleanup strategy

## Next Steps

1. [ ] Set up dev environment
2. [ ] Build agent prototype
3. [ ] Design database schema
4. [ ] Create API contracts

---

Questions: himanshu.dixit@nexod.ai
