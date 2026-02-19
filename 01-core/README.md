# Core Architecture

**Owner**: Himanshu Dixit

## System Overview

Agent-native platform. Users define workflows, agents execute autonomously.

## Tech Stack

### Frontend
- **Next.js 14 App Router** - Server Components, minimal JS
- **TypeScript strict** - Compile-time safety
- **Tailwind CSS** - Utility-first styling
- **Zustand + React Query** - State management
- **Figma MCP** - Rapid prototyping pipeline

### Backend
- **FastAPI** - Async Python, Pydantic validation
- **SQLAlchemy 2.0** - Async ORM
- **Alembic** - Database migrations
- **Celery + Redis** - Background jobs
- **Pinecone** - Vector database

### Agent Framework

**CrewAI** - Multi-agent orchestration
```python
researcher = Agent(
    role='Researcher',
    goal='Gather accurate information',
    tools=[search_tool, browse_tool],
    memory=True
)

crew = Crew(
    agents=[researcher, analyst, writer],
    tasks=[research_task, analyze_task, write_task],
    process=Process.sequential,
    memory=True
)

result = crew.kickoff(inputs={'topic': 'AI trends'})
```

**LangGraph** - State machines for complex flows
```python
from langgraph.graph import StateGraph, END

class WorkflowState(TypedDict):
    input: str
    research: str
    approved: bool
    output: str

workflow = StateGraph(WorkflowState)

# Nodes
workflow.add_node("research", research_node)
workflow.add_node("human_approval", approval_node)
workflow.add_node("generate", generate_node)

# Edges
workflow.add_edge("research", "human_approval")
workflow.add_conditional_edges(
    "human_approval",
    lambda state: "generate" if state["approved"] else END
)

app = workflow.compile()
```

**Vector Store (Pinecone)**
```python
# RAG for agents
index = pc.Index("knowledge-base")

# Store embeddings
index.upsert(vectors=[{
    "id": doc_id,
    "values": embedding,
    "metadata": {"source": "documentation"}
}])

# Query
results = index.query(
    vector=query_embedding,
    top_k=5,
    filter={"source": {"$eq": "documentation"}}
)
```

## Architecture Decisions

| Decision | Rationale |
|----------|-----------|
| FastAPI over Django | Async for agents, lighter weight |
| Next.js 14 App Router | Server Components, edge rendering |
| CrewAI + LangGraph | Different agent patterns needed |
| PostgreSQL | ACID, JSONB for agent state |
| Pinecone | Managed vector ops, scales |
| Clerk | Best auth DX, webhooks |

## Data Flow

```
User Request
    ↓
Vercel Edge (Next.js)
    ↓
API Route → FastAPI
    ↓
Agent Orchestrator
    ├─→ CrewAI (multi-agent)
    ├─→ LangGraph (state machine)
    └─→ Direct LLM call
    ↓
PostgreSQL (state)
Redis (cache/sessions)
Pinecone (vectors)
    ↓
Response
```

## Database Schema

### Core Tables
```sql
-- Users (via Clerk webhook)
users (
    id uuid primary key,
    email text unique,
    created_at timestamp
)

-- Workspaces (multi-tenant)
workspaces (
    id uuid primary key,
    name text,
    owner_id uuid references users(id)
)

-- Agent Sessions
agent_sessions (
    id uuid primary key,
    workspace_id uuid references workspaces(id),
    agent_type text,
    status text,
    state jsonb,  -- LangGraph checkpoint
    created_at timestamp
)

-- Agent Logs
agent_logs (
    id uuid primary key,
    session_id uuid references agent_sessions(id),
    step text,
    output text,
    timestamp timestamp
)

-- Vector Metadata
vector_metadata (
    id text primary key,  -- Pinecone ID
    workspace_id uuid,
    content_type text,
    source text,
    created_at timestamp
)
```

## Performance Targets

| Metric | Target | P95 |
|--------|--------|-----|
| API Response | <100ms | <300ms |
| Agent Step | <2s | <5s |
| Full Workflow | <10s | <30s |
| DB Query | <50ms | <100ms |
| Vector Search | <100ms | <200ms |

## Scalability Considerations

**Horizontal**:
- Stateless FastAPI containers
- Redis for shared state
- PostgreSQL read replicas

**Vertical**:
- Async everywhere
- Connection pooling
- Query optimization
- Caching layers

**Agent Specific**:
- Step checkpointing (resume on failure)
- Parallel agent execution
- Rate limiting per workspace
- Queue-based execution

## Security

- Row-level security in PostgreSQL
- Clerk JWT verification
- API rate limiting
- Input validation (Pydantic)
- No secrets in code

## Monitoring

- Sentry: Errors, performance
- Prometheus: Custom metrics
- Py-spy: Profiling
- Structured logging

## Roadmap

**Phase 1**: Foundation
- Auth, basic CRUD
- Agent hello-world
- Dev environment

**Phase 2**: AI Core
- CrewAI integration
- LangGraph workflows
- Vector search

**Phase 3**: Scale
- Performance optimization
- Advanced patterns
- Production hardening

## Open Questions

1. PWA vs React Native for mobile?
2. On-premise deployment strategy?
3. Multi-region data residency?

---

himanshu.dixit@nexod.ai
