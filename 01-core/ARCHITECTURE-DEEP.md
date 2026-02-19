# Platform Architecture Deep Dive

**Owner**: Himanshu Dixit  
**Status**: Living Document  
**Last Updated**: Feb 2026

## Table of Contents

1. [System Overview](#system-overview)
2. [Layer 1: Client Layer](#layer-1-client-layer)
3. [Layer 2: Edge Layer](#layer-2-edge-layer)
4. [Layer 3: Application Layer](#layer-3-application-layer)
5. [Layer 4: Agent Layer](#layer-4-agent-layer)
6. [Layer 5: Data Layer](#layer-5-data-layer)
7. [Layer 6: Infrastructure Layer](#layer-6-infrastructure-layer)
8. [Cross-Cutting Concerns](#cross-cutting-concerns)
9. [Service Interactions](#service-interactions)
10. [Data Flow Patterns](#data-flow-patterns)

---

## System Overview

### Architecture Philosophy

**Agent-Native Platform**: Every feature designed with autonomous agents as first-class citizens.

**Principles**:
- Stateless where possible, stateful where necessary
- Event-driven async communication
- Type safety at all boundaries
- Observability by default
- Fail fast, retry with backoff

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     CLIENT LAYER                            │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │  Web App │  │ Mobile   │  │ API      │  │ Webhook  │   │
│  │ (Next.js)│  │ (PWA)    │  │ Clients  │  │ Receivers│   │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘  └────┬─────┘   │
└───────┼─────────────┼─────────────┼─────────────┼───────────┘
        │             │             │             │
        └─────────────┴──────┬──────┴─────────────┘
                             │
┌────────────────────────────┼────────────────────────────────┐
│                     EDGE LAYER                             │
│  ┌─────────────────────────┼──────────────────────────┐    │
│  │              Vercel Edge Network                   │    │
│  │  ┌──────────────┐  ┌──────────────┐              │    │
│  │  │ Edge Func    │  │ Middleware   │              │    │
│  │  │ (Auth, Geo)  │  │ (Rate Limit) │              │    │
│  │  └──────────────┘  └──────────────┘              │    │
│  └─────────────────────────┼──────────────────────────┘    │
└────────────────────────────┼────────────────────────────────┘
                             │
┌────────────────────────────┼────────────────────────────────┐
│                  APPLICATION LAYER                         │
│  ┌─────────────────────────┼──────────────────────────┐    │
│  │              AWS Application Load Balancer          │    │
│  └─────────────────────────┼──────────────────────────┘    │
│                            │                                │
│  ┌─────────────────────────┴──────────────────────────┐    │
│  │                 ECS Fargate Cluster                │    │
│  │  ┌──────────────┐  ┌──────────────┐              │    │
│  │  │ API Service  │  │ Agent Worker │              │    │
│  │  │ (FastAPI)    │  │ (Celery)     │              │    │
│  │  └──────────────┘  └──────────────┘              │    │
│  └────────────────────────────────────────────────────┘    │
└────────────────────────────────────────────────────────────┘
                             │
┌────────────────────────────┼────────────────────────────────┐
│                     AGENT LAYER                            │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐     │
│  │   CrewAI     │  │  LangGraph   │  │   Direct     │     │
│  │ Orchestrator │  │   Engine     │  │   LLM        │     │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘     │
│         │                 │                 │              │
│         └─────────────────┼─────────────────┘              │
│                           │                                │
│  ┌──────────────┐  ┌──────┴───────┐  ┌──────────────┐     │
│  │   Tools      │  │   Memory     │  │   Vector     │     │
│  │ (Search, etc)│  │   Store      │  │   Search     │     │
│  └──────────────┘  └──────────────┘  └──────────────┘     │
└───────────────────────────┼────────────────────────────────┘
                            │
┌───────────────────────────┼────────────────────────────────┐
│                      DATA LAYER                            │
│  ┌──────────────┐  ┌──────┴───────┐  ┌──────────────┐     │
│  │ PostgreSQL   │  │    Redis     │  │  Pinecone    │     │
│  │ (Primary)    │  │   (Cache)    │  │  (Vectors)   │     │
│  └──────────────┘  └──────────────┘  └──────────────┘     │
└───────────────────────────┼────────────────────────────────┘
                            │
┌───────────────────────────┼────────────────────────────────┐
│                INFRASTRUCTURE LAYER                        │
│  ┌──────────────┐  ┌──────┴───────┐  ┌──────────────┐     │
│  │   GitHub     │  │   Sentry     │  │ Prometheus/  │     │
│  │   Actions    │  │              │  │   Grafana    │     │
│  └──────────────┘  └──────────────┘  └──────────────┘     │
└────────────────────────────────────────────────────────────┘
```

---

## Layer 1: Client Layer

### Web Application (Next.js 14)

**Architecture**: App Router with Server Components

```
app/
├── (marketing)/           # Marketing pages (static)
├── (dashboard)/           # Authenticated app
│   ├── layout.tsx         # Auth wrapper
│   ├── page.tsx           # Dashboard home
│   ├── agents/
│   │   ├── page.tsx       # Agent list (Server)
│   │   └── [id]/
│   │       ├── page.tsx   # Agent detail (Server)
│   │       └── run/
│   │           └── page.tsx # Run agent (Client)
│   ├── workflows/
│   └── settings/
├── api/                   # API routes (edge functions)
│   ├── webhooks/
│   │   └── clerk/         # Auth webhooks
│   └── agents/
│       └── [id]/run/      # Trigger agent run
└── layout.tsx             # Root layout
```

**Rendering Strategy**:

| Route | Strategy | Why |
|-------|----------|-----|
| `/` | Static | Marketing, cacheable |
| `/agents` | Server | Dynamic data, SEO |
| `/agents/[id]/run` | Client | Interactivity, WebSocket |
| `/api/*` | Edge | Low latency, auth |

**State Management**:

```typescript
// Server State (React Query)
const { data: agents } = useQuery({
  queryKey: ['agents'],
  queryFn: fetchAgents,
  staleTime: 5 * 60 * 1000,
})

// Client State (Zustand)
const useAgentStore = create<AgentStore>()(
  devtools(
    persist(
      (set, get) => ({
        selectedAgent: null,
        runningAgents: new Set(),
        setSelectedAgent: (agent) => set({ selectedAgent: agent }),
        addRunningAgent: (id) => set((state) => ({
          runningAgents: new Set([...state.runningAgents, id])
        })),
      }),
      { name: 'agent-storage' }
    )
  )
)

// URL State (for sharing)
const router = useRouter()
const params = useSearchParams()
```

### Progressive Web App (PWA)

**Features**:
- Offline support with Service Worker
- Background sync for agent runs
- Push notifications for agent completion
- Add to home screen

**Implementation**:
```typescript
// app/manifest.ts
export default function manifest(): MetadataRoute.Manifest {
  return {
    name: 'Nexod Platform',
    short_name: 'Nexod',
    start_url: '/',
    display: 'standalone',
    background_color: '#ffffff',
    theme_color: '#000000',
  }
}
```

### API Clients

**External Integration**:
```python
# REST API for third parties
@app.get("/api/v1/agents/{agent_id}/runs")
async def list_agent_runs(
    agent_id: str,
    api_key: str = Header(...),
    db: AsyncSession = Depends(get_db)
):
    # Verify API key
    workspace = await verify_api_key(db, api_key)
    # Return runs
    return await get_agent_runs(db, agent_id, workspace.id)
```

---

## Layer 2: Edge Layer

### Vercel Edge Network

**Edge Functions**:

```typescript
// middleware.ts
import { authMiddleware } from '@clerk/nextjs'

export default authMiddleware({
  publicRoutes: ['/', '/login', '/signup'],
  beforeAuth: (req) => {
    // Rate limiting check
    const ip = req.ip ?? '127.0.0.1'
    return checkRateLimit(ip)
  },
})

export const config = {
  matcher: ['/((?!.*\\.).*)'],
}
```

**Geographic Distribution**:
- Static assets cached at edge
- API routes served from nearest region
- Dynamic content streamed

### Middleware Stack

1. **Authentication** (Clerk)
   - JWT validation
   - Session refresh
   - Redirect to login if unauthorized

2. **Rate Limiting** (Redis)
   ```typescript
   // Upstash Redis for edge
   import { Ratelimit } from '@upstash/ratelimit'
   
   const ratelimit = new Ratelimit({
     redis: Redis.fromEnv(),
     limiter: Ratelimit.slidingWindow(10, '10 s'),
   })
   ```

3. **Geo Routing**
   - Route to nearest backend region
   - GDPR compliance for EU users

---

## Layer 3: Application Layer

### API Service (FastAPI)

**Application Structure**:
```
app/
├── __init__.py
├── main.py                 # App factory
├── config.py              # Pydantic settings
├── dependencies.py        # DI container
├── middleware/
│   ├── __init__.py
│   ├── logging.py         # Request logging
│   ├── tracing.py         # OpenTelemetry
│   └── errors.py          # Exception handlers
├── routers/
│   ├── __init__.py
│   ├── auth.py            # Authentication
│   ├── agents.py          # Agent CRUD
│   ├── runs.py            # Agent execution
│   ├── workflows.py       # Workflow management
│   └── webhooks.py        # Incoming webhooks
├── services/
│   ├── __init__.py
│   ├── agent_service.py   # Business logic
│   ├── run_service.py     # Execution logic
│   └── workflow_service.py
├── agents/                # Agent implementations
│   ├── __init__.py
│   ├── crew_factory.py    # CrewAI builder
│   ├── graph_factory.py   # LangGraph builder
│   └── tools/
│       ├── __init__.py
│       ├── search.py
│       ├── database.py
│       └── api.py
├── models/                # SQLAlchemy
├── schemas/               # Pydantic
└── tasks/                 # Celery tasks
```

**Dependency Injection**:

```python
# dependencies.py
from typing import AsyncGenerator
from fastapi import Depends, HTTPException, status
from sqlalchemy.ext.asyncio import AsyncSession
from app.database import async_session_maker
from app.services.agent_service import AgentService
from app.agents.crew_factory import CrewFactory

async def get_db() -> AsyncGenerator[AsyncSession, None]:
    async with async_session_maker() as session:
        try:
            yield session
            await session.commit()
        except Exception:
            await session.rollback()
            raise
        finally:
            await session.close()

async def get_current_user(
    token: str = Depends(oauth2_scheme),
    db: AsyncSession = Depends(get_db)
) -> User:
    user = await verify_token(db, token)
    if not user:
        raise HTTPException(
            status_code=status.HTTP_401_UNAUTHORIZED,
            detail="Invalid authentication credentials"
        )
    return user

async def get_agent_service(
    db: AsyncSession = Depends(get_db)
) -> AgentService:
    return AgentService(db)

# Usage in router
@router.post("/agents", response_model=AgentResponse)
async def create_agent(
    data: AgentCreate,
    service: AgentService = Depends(get_agent_service),
    current_user: User = Depends(get_current_user)
):
    return await service.create_agent(data, current_user.workspace_id)
```

### Agent Worker (Celery)

**Worker Configuration**:
```python
# celery_app.py
from celery import Celery
from celery.signals import task_prerun, task_postrun

celery_app = Celery(
    'agent_worker',
    broker='redis://redis:6379/0',
    backend='redis://redis:6379/0',
    include=['app.tasks.agent_tasks']
)

celery_app.conf.update(
    task_serializer='json',
    accept_content=['json'],
    result_serializer='json',
    timezone='UTC',
    enable_utc=True,
    task_track_started=True,
    task_time_limit=3600,  # 1 hour max
    worker_prefetch_multiplier=1,  # Fair scheduling
    broker_connection_retry_on_startup=True,
)

@task_prerun.connect
def task_started_handler(sender=None, task_id=None, task=None, args=None, kwargs=None, **extras):
    logger.info(f"Task {task_id} started: {task.name}")

@task_postrun.connect
def task_completed_handler(sender=None, task_id=None, task=None, retval=None, state=None, **extras):
    logger.info(f"Task {task_id} completed with state: {state}")
```

**Agent Execution Task**:
```python
# tasks/agent_tasks.py
from celery import shared_task
from celery.exceptions import MaxRetriesExceededError

@shared_task(
    bind=True,
    max_retries=3,
    default_retry_delay=60,
    autoretry_for=(Exception,),
    retry_backoff=True,
    retry_backoff_max=600,
)
def run_agent_task(self, agent_id: str, inputs: dict, workspace_id: str):
    """Execute agent workflow"""
    try:
        # Update status to running
        update_run_status(agent_id, "running")
        
        # Load agent configuration
        agent_config = load_agent_config(agent_id)
        
        # Execute based on type
        if agent_config.type == "crew":
            result = execute_crew_agent(agent_config, inputs)
        elif agent_config.type == "graph":
            result = execute_graph_agent(agent_config, inputs)
        else:
            result = execute_direct_llm(agent_config, inputs)
        
        # Store result
        store_run_result(agent_id, result)
        
        # Send webhook notification
        send_webhook.delay(workspace_id, "agent.completed", {
            "agent_id": agent_id,
            "status": "completed",
            "result": result
        })
        
        return result
        
    except MaxRetriesExceededError:
        update_run_status(agent_id, "failed")
        send_webhook.delay(workspace_id, "agent.failed", {
            "agent_id": agent_id,
            "error": "Max retries exceeded"
        })
        raise
    except Exception as exc:
        logger.error(f"Agent execution failed: {exc}", exc_info=True)
        # Retry with exponential backoff
        raise self.retry(exc=exc)
```

---

## Layer 4: Agent Layer

### CrewAI Orchestrator

**Agent Definition**:
```python
# agents/crew_factory.py
from crewai import Agent, Task, Crew, Process
from crewai.tools import BaseTool

class CrewFactory:
    """Factory for building CrewAI workflows"""
    
    @staticmethod
    def create_research_crew(config: dict) -> Crew:
        # Define agents
        researcher = Agent(
            role='Research Analyst',
            goal='Gather comprehensive information on topics',
            backstory='''You are an expert research analyst with 10+ years 
            of experience. You excel at finding accurate information from 
            diverse sources and synthesizing it into actionable insights.''',
            tools=[SearchTool(), BrowseTool()],
            llm=config.get('llm', 'gpt-4'),
            memory=True,
            verbose=True,
            allow_delegation=False,
        )
        
        analyst = Agent(
            role='Data Analyst',
            goal='Analyze research findings and identify patterns',
            backstory='''You are a skilled data analyst who can extract 
            meaningful insights from complex information.''',
            tools=[AnalysisTool()],
            memory=True,
        )
        
        writer = Agent(
            role='Technical Writer',
            goal='Create clear, structured documentation',
            backstory='''You are an experienced technical writer who 
            transforms complex information into accessible content.''',
            memory=True,
        )
        
        # Define tasks
        research_task = Task(
            description='Research {topic} thoroughly. Find at least 5 reliable sources.',
            expected_output='Comprehensive research notes with citations',
            agent=researcher,
        )
        
        analysis_task = Task(
            description='Analyze the research findings and identify key themes',
            expected_output='Structured analysis with key insights',
            agent=analyst,
            context=[research_task],
        )
        
        writing_task = Task(
            description='Write a comprehensive report based on the analysis',
            expected_output='Final report in markdown format',
            agent=writer,
            context=[analysis_task],
            output_callback=save_report_callback,
        )
        
        # Create crew
        crew = Crew(
            agents=[researcher, analyst, writer],
            tasks=[research_task, analysis_task, writing_task],
            process=Process.sequential,
            memory=True,
            cache=True,
            max_rpm=100,  # Rate limiting
        )
        
        return crew
```

### LangGraph Engine

**State Machine Definition**:
```python
# agents/graph_factory.py
from langgraph.graph import StateGraph, END
from langgraph.checkpoint import PostgresCheckpoint
from typing import TypedDict, Annotated
import operator

class AgentState(TypedDict):
    """State schema for LangGraph workflows"""
    messages: Annotated[list, operator.add]
    next_step: str
    context: dict
    approved: bool
    output: str
    error: Optional[str]

def create_approval_workflow(config: dict) -> StateGraph:
    """Create approval workflow with human-in-the-loop"""
    
    # Build graph
    workflow = StateGraph(AgentState)
    
    # Add nodes
    workflow.add_node("research", research_node)
    workflow.add_node("draft", draft_node)
    workflow.add_node("review", review_node)
    workflow.add_node("human_approval", human_approval_node)
    workflow.add_node("revise", revise_node)
    workflow.add_node("publish", publish_node)
    workflow.add_node("error_handler", error_handler_node)
    
    # Define edges
    workflow.set_entry_point("research")
    
    # Research -> Draft (always)
    workflow.add_edge("research", "draft")
    
    # Draft -> Review (always)
    workflow.add_edge("draft", "review")
    
    # Review -> Decision (conditional)
    workflow.add_conditional_edges(
        "review",
        lambda state: "human_approval" if state["score"] > 0.8 else "revise",
        {
            "human_approval": "human_approval",
            "revise": "revise"
        }
    )
    
    # Human approval -> Decision
    workflow.add_conditional_edges(
        "human_approval",
        lambda state: "publish" if state["approved"] else "revise",
        {
            "publish": "publish",
            "revise": "revise"
        }
    )
    
    # Revise -> Review (loop until good)
    workflow.add_edge("revise", "review")
    
    # Publish -> End
    workflow.add_edge("publish", END)
    
    # Error handling
    for node in ["research", "draft", "review"]:
        workflow.add_conditional_edges(
            node,
            lambda state: "error" if state.get("error") else "continue",
            {
                "error": "error_handler",
                "continue": None  # Continue to next defined edge
            }
        )
    
    # Error handler -> End
    workflow.add_edge("error_handler", END)
    
    # Compile with checkpointing
    checkpointer = PostgresCheckpoint(
        conn_string=config['database_url'],
        table_name="agent_checkpoints"
    )
    
    app = workflow.compile(checkpointer=checkpointer)
    
    return app

def research_node(state: AgentState) -> AgentState:
    """Research node implementation"""
    try:
        # Execute research
        result = research_tool.run(state["context"]["topic"])
        
        return {
            **state,
            "messages": [{"role": "assistant", "content": result}],
            "context": {**state["context"], "research": result}
        }
    except Exception as e:
        return {
            **state,
            "error": str(e),
            "messages": [{"role": "error", "content": str(e)}]
        }

def human_approval_node(state: AgentState) -> AgentState:
    """Human approval node - interrupts for human input"""
    # This triggers an interrupt
    # Human reviews via UI
    # State resumes when approved/rejected
    
    return {
        **state,
        "__interrupt__": {
            "type": "approval",
            "message": "Please review and approve/reject",
            "data": state["output"]
        }
    }
```

### Tool Registry

```python
# agents/tools/registry.py
from typing import Dict, Type
from crewai.tools import BaseTool

class ToolRegistry:
    """Registry for agent tools"""
    
    _tools: Dict[str, Type[BaseTool]] = {}
    
    @classmethod
    def register(cls, name: str, tool_class: Type[BaseTool]):
        cls._tools[name] = tool_class
    
    @classmethod
    def get(cls, name: str) -> BaseTool:
        if name not in cls._tools:
            raise ValueError(f"Tool {name} not found")
        return cls._tools[name]()
    
    @classmethod
    def list_tools(cls) -> list[str]:
        return list(cls._tools.keys())

# Register tools
from .search import SearchTool
from .database import DatabaseQueryTool
from .api import APICallTool

ToolRegistry.register("search", SearchTool)
ToolRegistry.register("database_query", DatabaseQueryTool)
ToolRegistry.register("api_call", APICallTool)
```

---

## Layer 5: Data Layer

### PostgreSQL Schema

**Core Tables**:
```sql
-- Workspaces (multi-tenant)
CREATE TABLE workspaces (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL,
    slug VARCHAR(255) UNIQUE NOT NULL,
    plan VARCHAR(50) DEFAULT 'free',
    settings JSONB DEFAULT '{}',
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Users (synced from Clerk)
CREATE TABLE users (
    id UUID PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    first_name VARCHAR(100),
    last_name VARCHAR(100),
    avatar_url TEXT,
    workspace_id UUID REFERENCES workspaces(id),
    role VARCHAR(50) DEFAULT 'member',
    last_active_at TIMESTAMPTZ,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- API Keys
CREATE TABLE api_keys (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    workspace_id UUID REFERENCES workspaces(id) ON DELETE CASCADE,
    name VARCHAR(255),
    key_hash VARCHAR(255) NOT NULL,
    scopes TEXT[],
    last_used_at TIMESTAMPTZ,
    expires_at TIMESTAMPTZ,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    revoked_at TIMESTAMPTZ
);

-- Agents
CREATE TABLE agents (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    workspace_id UUID REFERENCES workspaces(id) ON DELETE CASCADE,
    name VARCHAR(255) NOT NULL,
    description TEXT,
    type VARCHAR(50) NOT NULL, -- 'crew', 'graph', 'direct'
    config JSONB NOT NULL, -- Agent configuration
    tools TEXT[], -- Tool names
    is_active BOOLEAN DEFAULT true,
    created_by UUID REFERENCES users(id),
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Agent Runs
CREATE TABLE agent_runs (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    agent_id UUID REFERENCES agents(id) ON DELETE CASCADE,
    workspace_id UUID REFERENCES workspaces(id),
    status VARCHAR(50) NOT NULL, -- 'pending', 'running', 'completed', 'failed'
    inputs JSONB,
    outputs JSONB,
    error_message TEXT,
    started_at TIMESTAMPTZ,
    completed_at TIMESTAMPTZ,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Agent Logs (partitioned by month)
CREATE TABLE agent_logs (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    run_id UUID REFERENCES agent_runs(id) ON DELETE CASCADE,
    step_number INTEGER,
    agent_name VARCHAR(255),
    action VARCHAR(255),
    input TEXT,
    output TEXT,
    latency_ms INTEGER,
    timestamp TIMESTAMPTZ DEFAULT NOW()
) PARTITION BY RANGE (timestamp);

-- Create partitions
CREATE TABLE agent_logs_2026_01 PARTITION OF agent_logs
    FOR VALUES FROM ('2026-01-01') TO ('2026-02-01');

-- LangGraph Checkpoints
CREATE TABLE agent_checkpoints (
    thread_id VARCHAR(255) PRIMARY KEY,
    agent_id UUID REFERENCES agents(id) ON DELETE CASCADE,
    checkpoint JSONB NOT NULL,
    created_at TIMESTAMPTZ DEFAULT NOW(),
    updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Vector Metadata (maps to Pinecone)
CREATE TABLE vector_metadata (
    id TEXT PRIMARY KEY, -- Pinecone ID
    workspace_id UUID REFERENCES workspaces(id) ON DELETE CASCADE,
    agent_id UUID REFERENCES agents(id) ON DELETE SET NULL,
    content_type VARCHAR(50), -- 'document', 'conversation', 'knowledge'
    source VARCHAR(255),
    title VARCHAR(500),
    content_hash VARCHAR(64),
    metadata JSONB,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Webhook Subscriptions
CREATE TABLE webhooks (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    workspace_id UUID REFERENCES workspaces(id) ON DELETE CASCADE,
    url TEXT NOT NULL,
    events TEXT[], -- ['agent.started', 'agent.completed']
    secret VARCHAR(255), -- For HMAC signature
    is_active BOOLEAN DEFAULT true,
    last_triggered_at TIMESTAMPTZ,
    created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Indexes
CREATE INDEX idx_users_workspace ON users(workspace_id);
CREATE INDEX idx_agents_workspace ON agents(workspace_id);
CREATE INDEX idx_runs_agent ON agent_runs(agent_id);
CREATE INDEX idx_runs_status ON agent_runs(status);
CREATE INDEX idx_logs_run ON agent_logs(run_id);
CREATE INDEX idx_logs_timestamp ON agent_logs(timestamp);
CREATE INDEX idx_vector_workspace ON vector_metadata(workspace_id);
CREATE INDEX idx_vector_content ON vector_metadata USING GIN(content);
```

### Redis Data Structures

```python
# Session Cache
session:{user_id} -> hash { user_id, workspace_id, permissions, exp }

# Rate Limiting
rate_limit:{user_id}:{endpoint} -> string (counter with TTL)

# Agent State
agent:{agent_id}:state -> hash { status, progress, current_step, result }

# Queue
# Using Redis as Celery broker
# Queues: default, high_priority, agents, webhooks

# Real-time
workspace:{workspace_id}:agents -> pub/sub channel
run:{run_id}:updates -> stream

# Cache
api:{cache_key} -> string (JSON)
query:{query_hash} -> string (result)
```

### Pinecone Index Design

```python
# Index configuration
index_name = "nexod-knowledge"
dimension = 1536  # OpenAI embeddings
metric = "cosine"
pod_type = "p1.x1"

# Namespace strategy
# namespace = workspace_id (isolation)

# Metadata fields
{
    "workspace_id": "uuid",
    "agent_id": "uuid",
    "content_type": "document|conversation|knowledge",
    "source": "url or file",
    "created_at": "timestamp",
    "title": "string"
}
```

---

## Layer 6: Infrastructure Layer

### CI/CD Pipeline

```yaml
# .github/workflows/deploy.yml
name: Deploy

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    services:
      postgres:
        image: postgres:15
        env:
          POSTGRES_PASSWORD: postgres
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
        ports:
          - 5432:5432
      redis:
        image: redis:7
        ports:
          - 6379:6379
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'
      
      - name: Install backend deps
        run: |
          cd backend
          pip install -r requirements.txt
          pip install -r requirements-test.txt
      
      - name: Run backend tests
        run: |
          cd backend
          pytest --cov=app --cov-report=xml
        env:
          DATABASE_URL: postgresql+asyncpg://postgres:postgres@localhost:5432/test
          REDIS_URL: redis://localhost:6379/0
      
      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: '18'
      
      - name: Install frontend deps
        run: |
          cd frontend
          npm ci
      
      - name: Run frontend tests
        run: |
          cd frontend
          npm test -- --coverage
      
      - name: Type check
        run: |
          cd frontend
          npm run type-check
      
      - name: Lint
        run: |
          cd backend && ruff check .
          cd frontend && npm run lint

  deploy-staging:
    needs: test
    if: github.ref == 'refs/heads/develop'
    runs-on: ubuntu-latest
    
    steps:
      - name: Deploy to Staging
        run: |
          aws ecs update-service \
            --cluster staging \
            --service api \
            --force-new-deployment

  deploy-production:
    needs: test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    environment: production
    
    steps:
      - name: Deploy to Production
        run: |
          aws ecs update-service \
            --cluster production \
            --service api \
            --force-new-deployment
```

### Monitoring Stack

**Sentry Configuration**:
```python
# Full error tracking with context
sentry_sdk.init(
    dsn=settings.SENTRY_DSN,
    environment=settings.ENVIRONMENT,
    release=settings.GIT_SHA,
    traces_sample_rate=0.1,
    profiles_sample_rate=0.01,
    integrations=[
        FastApiIntegration(),
        SqlalchemyIntegration(),
        CeleryIntegration(),
    ],
    before_send=lambda event, hint: filter_pii(event),
)
```

**Prometheus Metrics**:
```python
# Custom metrics
AGENT_RUNS_TOTAL = Counter(
    'agent_runs_total',
    'Total agent executions',
    ['agent_type', 'status', 'workspace_id']
)

AGENT_RUN_DURATION = Histogram(
    'agent_run_duration_seconds',
    'Agent execution time',
    ['agent_type'],
    buckets=[1, 5, 10, 30, 60, 120, 300, 600]
)

DB_QUERY_DURATION = Histogram(
    'db_query_duration_seconds',
    'Database query time',
    ['query_type']
)
```

---

## Cross-Cutting Concerns

### Authentication Flow

```
1. User logs in via Clerk (frontend)
2. Clerk issues JWT
3. Frontend includes JWT in API requests
4. API validates JWT (Clerk SDK)
5. API extracts user_id, workspace_id
6. All queries filtered by workspace_id (RLS)
```

### Error Handling Strategy

```python
# Exception hierarchy
class AppError(Exception):
    """Base application error"""
    status_code = 500
    code = "internal_error"

class ValidationError(AppError):
    status_code = 400
    code = "validation_error"

class NotFoundError(AppError):
    status_code = 404
    code = "not_found"

class AgentError(AppError):
    status_code = 500
    code = "agent_execution_error"

# Global handler
@app.exception_handler(AppError)
async def app_error_handler(request: Request, exc: AppError):
    return JSONResponse(
        status_code=exc.status_code,
        content={
            "error": {
                "code": exc.code,
                "message": str(exc),
                "request_id": request.state.request_id
            }
        }
    )
```

### Logging Standards

```python
# Structured logging
logger.info(
    "agent_run_started",
    extra={
        "agent_id": str(agent_id),
        "workspace_id": str(workspace_id),
        "user_id": str(user_id),
        "inputs_hash": hash_inputs(inputs),
        "correlation_id": correlation_id
    }
)
```

---

## Service Interactions

### Sequence: Agent Execution

```
User -> Next.js: Click "Run Agent"
Next.js -> FastAPI: POST /agents/{id}/runs
FastAPI -> PostgreSQL: Create run record
FastAPI -> Redis: Queue task (Celery)
FastAPI -> Next.js: Return run_id (202 Accepted)

Celery Worker -> PostgreSQL: Update status (running)
Celery Worker -> CrewAI: Execute workflow
Celery Worker -> Pinecone: Vector search (RAG)
Celery Worker -> PostgreSQL: Store results
Celery Worker -> Redis: Publish update
Celery Worker -> Webhook: POST to workspace URL

Redis -> Next.js: SSE update (real-time)
User -> Next.js: See results
```

### API Contracts

**Agent Run Endpoint**:
```yaml
POST /api/v1/agents/{agent_id}/runs
Request:
  body:
    inputs:
      topic: "AI trends 2026"
      style: "technical"
    
Response (202 Accepted):
  body:
    run_id: "uuid"
    status: "pending"
    estimated_duration: 30
    
GET /api/v1/runs/{run_id}
Response (200 OK):
  body:
    id: "uuid"
    status: "completed"
    outputs:
      report: "markdown content"
      sources: [...]
    completed_at: "2026-02-19T10:00:00Z"
```

---

## Data Flow Patterns

### Pattern 1: Event Sourcing (Agent Logs)
- Every agent action logged as event
- Reconstruct state from event stream
- Audit trail, replay capability

### Pattern 2: CQRS (Agent Runs)
- Command: Start run (write to PostgreSQL)
- Query: Get run status (read from Redis cache)
- Eventual consistency acceptable

### Pattern 3: Saga (Multi-Step Workflows)
- LangGraph manages distributed transaction
- Each step has compensation (rollback)
- Checkpoint on each step

---

himanshu.dixit@nexod.ai
