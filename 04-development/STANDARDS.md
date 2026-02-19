# Development Standards

**Enforced by**: CI/CD, code review

## Code Quality Gates

- [ ] Type checking (mypy, TypeScript strict)
- [ ] Linting (ruff, ESLint)
- [ ] Formatting (black, prettier)
- [ ] Tests 80%+ coverage
- [ ] Security scan
- [ ] No secrets

## Backend

### FastAPI Structure
```
app/
├── main.py              # App factory
├── config.py            # Settings
├── dependencies.py      # DI
├── database.py          # DB connection
├── models/              # SQLAlchemy
├── schemas/             # Pydantic
├── routers/             # API routes
├── services/            # Business logic
├── agents/              # CrewAI, LangGraph
├── tasks/               # Celery
└── tests/
```

### Agent Pattern
```python
# agents/research_crew.py
from crewai import Agent, Task, Crew

researcher = Agent(
    role='Researcher',
    goal='Find accurate information',
    backstory='Expert researcher with 10 years experience',
    tools=[search_tool],
    verbose=True
)

task = Task(
    description='Research {topic}',
    expected_output='3-paragraph summary',
    agent=researcher
)

crew = Crew(agents=[researcher], tasks=[task])
result = crew.kickoff(inputs={'topic': 'AI agents'})
```

### Type Hints
```python
# Good
def process_agent_result(result: AgentResult) -> dict[str, Any]:
    return result.to_dict()

# Bad
def process_agent_result(result):
    return result.to_dict()
```

### Error Handling
```python
class AgentError(Exception):
    pass

@app.exception_handler(AgentError)
async def agent_error_handler(request: Request, exc: AgentError):
    return JSONResponse(
        status_code=500,
        content={'error': 'Agent failed', 'detail': str(exc)}
    )
```

## Frontend

### Structure
```
app/
├── layout.tsx
├── page.tsx
├── components/
│   ├── ui/              # Button, Input
│   ├── features/        # AgentConsole
│   └── agents/          # Agent-specific UI
├── hooks/
├── stores/
└── lib/
```

### Component Pattern
```typescript
// components/agents/AgentConsole.tsx
'use client'

import { useQuery } from '@tanstack/react-query'

interface AgentConsoleProps {
  agentId: string
}

export function AgentConsole({ agentId }: AgentConsoleProps) {
  const { data, isLoading } = useQuery({
    queryKey: ['agent', agentId],
    queryFn: () => fetchAgentStatus(agentId),
  })
  
  if (isLoading) return <AgentSkeleton />
  
  return (
    <div className="space-y-4">
      <AgentStatus status={data?.status} />
      <AgentLogs logs={data?.logs} />
    </div>
  )
}
```

### Reusable Components

**Button**:
```typescript
interface ButtonProps extends ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: 'primary' | 'secondary' | 'danger'
  isLoading?: boolean
}

export const Button = forwardRef<HTMLButtonElement, ButtonProps>(
  ({ variant = 'primary', isLoading, children, ...props }, ref) => (
    <button
      ref={ref}
      className={cn(
        'rounded font-medium',
        variant === 'primary' && 'bg-blue-600 text-white',
        'disabled:opacity-50'
      )}
      disabled={isLoading || props.disabled}
      {...props}
    >
      {isLoading ? <Spinner /> : children}
    </button>
  )
)
```

### State Management

**Server State**:
```typescript
const { data } = useQuery({
  queryKey: ['agents'],
  queryFn: fetchAgents,
  staleTime: 5 * 60 * 1000,
})
```

**Client State**:
```typescript
const useAgentStore = create<AgentState>((set) => ({
  selectedAgent: null,
  setSelectedAgent: (agent) => set({ selectedAgent: agent }),
}))
```

## Agents

### CrewAI Pattern
```python
# Define agents with clear roles
crew = Crew(
    agents=[researcher, writer, reviewer],
    tasks=[research_task, write_task, review_task],
    process=Process.sequential,
    memory=True,  # Enable memory
    cache=True    # Cache results
)
```

### LangGraph Pattern
```python
from langgraph.graph import StateGraph

# Define state
class AgentState(TypedDict):
    messages: list
    next_step: str

# Build graph
workflow = StateGraph(AgentState)
workflow.add_node("research", research_node)
workflow.add_node("write", write_node)
workflow.add_conditional_edges("research", decide_next)
workflow.add_edge("write", END)

app = workflow.compile()
```

## Git Workflow

**Branches**:
```
feature/agent-workflow
bugfix/auth-redirect
hotfix/security
```

**Commits**:
```
feat: add research agent
crew: implement multi-agent workflow
fix: agent timeout issue
docs: update agent API
```

## Performance

**Backend**:
- API <200ms (p95)
- Agent <5s response
- DB queries <50ms
- Cache expensive operations

**Frontend**:
- FCP <1.8s
- LCP <2.5s
- Bundle <200KB
- Lazy load routes

## Security

- [ ] No secrets in code
- [ ] Input validation (Pydantic/Zod)
- [ ] SQL injection prevention
- [ ] XSS prevention
- [ ] Rate limiting
- [ ] HTTPS only
- [ ] Secure cookies

---

Questions: #engineering in Google Chat
