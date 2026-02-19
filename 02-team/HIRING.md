# Hiring Requirements

**Contact**: himanshu.dixit@nexod.ai  
**Positions**: 5 interns (2 backend, 2 frontend, 1 devops)

## Minimum Requirements

### All
- Python: functions, classes, async
- JavaScript/TypeScript: ES6+, async/await
- Git: branch, commit, PR
- SQL: JOINs, indexing
- REST APIs: methods, status codes
- Terminal basics

### Backend Interns (2)
- FastAPI/Flask, SQLAlchemy
- PostgreSQL queries
- pytest basics
- **Agent knowledge**: Understand LLMs, prompts, RAG basics
- **Challenge**: Build CRUD API + integrate OpenAI in 90 min

### Frontend Interns (2)
- React (hooks, components)
- TypeScript basics
- CSS (Flexbox, Grid)
- API integration
- **Figma MCP**: Willing to learn rapid prototyping
- **Challenge**: Build dashboard + integrate with API in 90 min

### DevOps Intern (1)
- Docker, CI/CD basics
- Linux, cloud basics (AWS)
- **Challenge**: Dockerize app + GitHub Actions in 90 min

## Agent-Focused Questions

**Q1: Explain RAG (Retrieval Augmented Generation)**
- Why: Core to our agent architecture
- Good: Retrieves docs, adds to prompt, generates answer
- Red flag: "It's just AI searching"

**Q2: What's the difference between CrewAI and LangGraph?**
- Why: We use both
- Good: CrewAI = multi-agent roles, LangGraph = state machines
- Red flag: "They're the same"

**Q3: How do you handle agent failures?**
- Why: Production agents fail
- Good: Retry logic, fallback responses, human handoff
- Red flag: "Agents don't fail"

**Q4: Design a research agent workflow**
- Why: Our core use case
- Good: Search → Analyze → Summarize → Store
- Red flag: One-shot prompt

## Standard Questions

### Backend

**Q1: Sync vs async Python?**
- Good: Sync blocks, async yields control
- Red flag: "Async is always faster"

**Q2: SQL injection prevention?**
- Good: Parameterized queries, ORM
- Red flag: "Sanitize inputs"

**Q3: N+1 query fix?**
- Good: Eager loading, selectinload
- Red flag: "Add indexes"

### Frontend

**Q1: Server vs Client Component?**
- Good: Server runs on server, Client in browser
- Red flag: "Server is always better"

**Q2: Form with 20 fields?**
- Good: React Hook Form + Zod
- Red flag: "useState for each"

**Q3: App slow, how debug?**
- Good: Profiler, bundle analyzer
- Red flag: "Add useMemo everywhere"

### General

**Q1: Production bug, process?**
- Good: Reproduce locally, check logs, fix, test, deploy
- Red flag: "Fix directly in prod"

## Red Flags

- Can't explain REST basics
- Never used Git
- Copy-pastes without understanding
- Never asks questions
- Defensive about feedback

## Green Flags

- Side projects on GitHub
- Built something with CrewAI/LangChain
- Explains concepts simply
- Handles "I don't know" well
- Writes about learning

## Technical Challenges

**Backend**:
```
Build FastAPI app with:
- User CRUD endpoints
- OpenAI integration (simple prompt)
- Pydantic validation
- Error handling
- 2+ tests
- README
```

**Frontend**:
```
Build React dashboard with:
- List data from API
- Form to add items
- Loading/error states
- TypeScript
- Responsive
- 1+ test
```

**Scoring**:
- Working code: 30%
- Organization: 25%
- Error handling: 20%
- Tests: 15%
- Communication: 10%

## Compensation

- Stipend: Competitive
- Equipment: MacBook Pro
- Learning: $500 budget
- Conversion: Top 20% get offers

## Apply

Email: himanshu.dixit@nexod.ai  
Subject: `[Internship] Backend/Frontend/DevOps - [Name]`

Attach:
1. Resume
2. GitHub link
3. Deployed project
4. Start date

**No cover letters.**

---

Last updated: Feb 2026
