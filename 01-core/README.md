# Core Architecture

**Owner**: Himanshu Dixit

## The Short Version

Agent platform. Users define workflows, agents execute. The platform handles orchestration, state, memory, and tool execution.

## Tech Stack

### Frontend
- **Next.js 14 App Router** - Server Components mean less JS sent to client. That's the main reason.
- **TypeScript strict** - Compile-time errors > runtime errors. Can't ship bad types.
- **Tailwind CSS** - Utility classes. No fighting with CSS specificity.
- **Zustand + React Query** - Zustand for UI state, React Query for server state. Simple.
- **Figma MCP** - We prototype fast. Design to component in hours, not days.

### Backend
- **FastAPI** - Async-first. Dependency injection that's actually useful. OpenAPI generated.
- **SQLAlchemy 2.0** - Async ORM. The 2.0 rewrite cleaned up a lot of mess.
- **Alembic** - Database migrations. Version control for schema.
- **Celery + Redis** - Background jobs. Long-running agent tasks, notifications, webhooks.
- **Pinecone** - Vector store. RAG for agent memory.

### Agent Framework

**CrewAI** - Multi-agent orchestration. Three agents working together on one task.

```python
researcher = Agent(
    role='Research Analyst',
    goal='Gather accurate information',
    tools=[search_tool, browse_tool],
    memory=True
)

crew = Crew(
    agents=[researcher, analyst, writer],
    tasks=[research_task, analyze_task, write_task],
    process=Process.sequential
)

result = crew.kickoff(inputs={'topic': 'AI trends'})
```

**LangGraph** - State machines. When you need conditional logic, human checkpoints, and resume-on-fail.

```python
from langgraph.graph import StateGraph, END

class WorkflowState(TypedDict):
    input: str
    research: str
    approved: bool
    output: str

workflow = StateGraph(WorkflowState)
workflow.add_node("research", research_node)
workflow.add_node("human_approval", approval_node)
workflow.add_node("generate", generate_node)
workflow.add_edge("research", "human_approval")
workflow.add_conditional_edges(
    "human_approval",
    lambda state: "generate" if state["approved"] else END
)

app = workflow.compile()
```

**Vector Store (Pinecone)** - RAG. Agents need context beyond their training.

```python
index = pc.Index("knowledge-base")
index.upsert(vectors=[{
    "id": doc_id,
    "values": embedding,
    "metadata": {"source": "documentation"}
}])
results = index.query(vector=query_embedding, top_k=5)
```

## Why These Decisions

| Decision | Reasoning |
|----------|-----------|
| FastAPI over Django | Async is built-in, not bolted on. Don't need Django's ORM since we have SQLAlchemy. |
| Next.js 14 App Router | Server Components cut bundle size significantly. RSC is the future of React. |
| CrewAI + LangGraph | They solve different problems. Use the right tool. |
| PostgreSQL | ACID compliance matters. Agent runs, billing, user data - all transactional. JSONB when we need flexibility. |
| Pinecone | Managed service. Don't want to deal with vector DB ops. |
| Clerk | Best auth DX. Pre-built components. Webhook sync keeps our DB honest. |

## How Data Flows

```
User Request
    ↓
Vercel Edge (Next.js)
    ↓
API Route → FastAPI
    ↓
Agent Orchestrator
    ├─→ CrewAI (multi-agent crew)
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

Core tables. We're building incrementally.

```sql
-- Workspaces (multi-tenant)
workspaces (
    id uuid primary key,
    name text,
    slug text unique,
    settings jsonb,
    created_at timestamptz
)

-- Users (synced from Clerk)
users (
    id uuid primary key,
    email text unique,
    workspace_id uuid references workspaces(id),
    role text default 'member'
)

-- Agents
agents (
    id uuid primary key,
    workspace_id uuid references workspaces(id),
    name text,
    type text,  -- 'crew', 'graph', 'direct'
    config jsonb,
    tools text[],
    created_by uuid references users(id)
)

-- Agent Runs
agent_runs (
    id uuid primary key,
    agent_id uuid references agents(id),
    workspace_id uuid,
    status text,  -- 'pending', 'running', 'completed', 'failed'
    inputs jsonb,
    outputs jsonb,
    started_at timestamptz,
    completed_at timestamptz
)
```

## Performance Targets

What we're aiming for. We'll measure, adjust.

| Metric | Target | Alert At |
|--------|--------|----------|
| API Response | <100ms | >300ms |
| Agent Step | <2s | >5s |
| Full Workflow | <10s | >30s |
| DB Query | <50ms | >100ms |
| Vector Search | <100ms | >200ms |

## Scaling Strategy

**Horizontal**:
- Stateless FastAPI containers
- Redis for shared state
- PostgreSQL read replicas when we need them

**Vertical**:
- Async everywhere
- Connection pooling (PgBouncer)
- Query optimization before caching
- Redis cache for expensive operations

**Agent-Specific**:
- LangGraph checkpointing - resume on failure
- Celery queue - don't block HTTP requests
- Per-workspace rate limiting - one bad actor doesn't kill everyone
- Streaming responses - users see progress

## Security

Basic stuff. We're not special targets but we're not stupid either.

- Row-level security - workspace isolation
- Clerk JWT - verify every request
- API rate limiting - 100 req/min per user
- Input validation - Pydantic, Zod
- No secrets in code - env vars only

## Monitoring

What we watch:

- **Sentry** - Errors, performance traces
- **Prometheus** - Custom metrics (agent runs, API latency)
- **Py-spy** - When something's slow, profile it
- **Structured logs** - JSON, searchable, correlated

---

himanshu.dixit@nexod.ai
