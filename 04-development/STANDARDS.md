# Development Standards

**Enforced by**: CI/CD, code review

## Code Quality

These are non-negotiable:

- **Type checking** - mypy, TypeScript strict. No "any".
- **Linting** - ruff, ESLint. Pass before merge.
- **Formatting** - black, prettier. Don't argue.
- **Tests** - 80% coverage minimum. No exceptions.
- **Secrets** - Never in code. Env vars only.

## Backend

### Structure
```
app/
├── main.py              # App factory
├── config.py           # Settings
├── dependencies.py      # DI container
├── database.py         # DB connection
├── models/             # SQLAlchemy
├── schemas/            # Pydantic
├── routers/           # API routes
├── services/           # Business logic
├── agents/             # CrewAI, LangGraph
├── tasks/              # Celery
└── tests/
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

### Agent Pattern
```python
researcher = Agent(
    role='Researcher',
    goal='Find accurate information',
    tools=[search_tool],
    memory=True
)

crew = Crew(agents=[researcher], tasks=[task])
result = crew.kickoff(inputs={'topic': 'AI agents'})
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
│   ├── features/       # AgentConsole
│   └── agents/          # Agent-specific
├── hooks/
├── stores/
└── lib/
```

### Component Pattern
```typescript
export function AgentConsole({ agentId }: { agentId: string }) {
  const { data, isLoading } = useQuery({
    queryKey: ['agent', agentId],
    queryFn: () => fetchAgentStatus(agentId),
  })
  
  if (isLoading) return <Skeleton />
  
  return <div>{data?.status}</div>
}
```

### State

**Server** - React Query:
```typescript
const { data } = useQuery({
  queryKey: ['agents'],
  queryFn: fetchAgents,
  staleTime: 5 * 60 * 1000,
})
```

**Client** - Zustand:
```typescript
const useAgentStore = create<AgentState>((set) => ({
  selectedAgent: null,
  setSelectedAgent: (agent) => set({ selectedAgent: agent }),
}))
```

## Git

### Branch Naming
```
feature/agent-workflow
bugfix/login-redirect
hotfix/security-patch
```

### Commits
```
feat: add research agent
fix: agent timeout handling
docs: update agent API
```

### PRs
- Link Trello card
- Include screenshots for UI
- Tests pass
- No merge conflicts

## Performance

**Backend**:
- API <200ms (p95)
- Agent <5s response
- DB queries <50ms

**Frontend**:
- Bundle <200KB
- Lazy load routes
- Measure, don't guess

## Security

- Input validation (Pydantic, Zod)
- SQL injection - use ORM, not raw strings
- XSS - React escapes by default
- Rate limiting - 100 req/min
- HTTPS - always

---

himanshu.dixit@nexod.ai
