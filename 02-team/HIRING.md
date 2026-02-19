# Hiring Requirements

**Contact**: himanshu.dixit@nexod.ai  
**Positions**: 5 interns (2 backend, 2 frontend, 1 devops)

## Must-Have Skills

### All Positions
- Python (functions, classes, async)
- JavaScript/TypeScript (ES6+, async/await)
- Git (branch, commit, PR)
- SQL (JOINs, indexing)
- REST APIs (methods, status codes)
- Terminal basics

### Backend Interns (2)
- FastAPI or Flask experience
- SQLAlchemy or similar ORM
- PostgreSQL queries
- pytest basics
- **Interview Challenge**: Build CRUD API in 90 min

### Frontend Interns (2)
- React (hooks, components)
- TypeScript basics
- CSS (Flexbox, Grid)
- API integration
- **Interview Challenge**: Build form with validation in 90 min

### DevOps Intern (1)
- Docker containers
- CI/CD basics
- Linux commands
- Cloud basics (AWS/GCP)
- **Interview Challenge**: Dockerize app + GitHub Actions in 90 min

## Interview Questions

### Backend

**Q1: What's the difference between sync and async Python?**
- **Why ask**: Core concept for FastAPI
- **Good answer**: Sync blocks, async yields control, event loop handles concurrency
- **Red flag**: "Async is just faster" (doesn't understand trade-offs)

**Q2: Explain SQL injection and how to prevent it.**
- **Why ask**: Security-critical
- **Good answer**: Malicious SQL via user input, use parameterized queries/ORM
- **Red flag**: "I sanitize inputs manually"

**Q3: You have N+1 query problem. How do you fix it?**
- **Why ask**: Common performance issue
- **Good answer**: Eager loading, selectinload, batch queries
- **Red flag**: "Add more indexes"

**Q4: What's the benefit of Alembic over raw SQL migrations?**
- **Why ask**: Database versioning
- **Good answer**: Version control, rollback, team coordination
- **Red flag**: "I just run SQL files"

**Q5: When would you use Celery?**
- **Why ask**: Async processing
- **Good answer**: Background jobs, long tasks, retry logic
- **Red flag**: "For making things faster"

### Frontend

**Q1: Server Component vs Client Component in Next.js 14?**
- **Why ask**: Core Next.js 14 concept
- **Good answer**: Server runs on server (no JS bundle), Client runs in browser
- **Red flag**: "Server is faster always"

**Q2: How do you handle form state with 20 fields?**
- **Why ask**: Real-world complexity
- **Good answer**: React Hook Form + Zod, validation, error handling
- **Red flag**: "useState for each field"

**Q3: Your app is slow. How do you debug?**
- **Why ask**: Performance mindset
- **Good answer**: Profiler, bundle analyzer, network tab, memoization
- **Red flag**: "Add useMemo everywhere"

**Q4: Explain useEffect cleanup function.**
- **Why ask**: Preventing memory leaks
- **Good answer**: Runs on unmount, cancel subscriptions/timers
- **Red flag**: "I don't need cleanup"

**Q5: How do you share state between components?**
- **Why ask**: State management
- **Good answer**: Context, Zustand, URL state, lift state up (with trade-offs)
- **Red flag**: "Redux for everything"

### General

**Q1: You find a bug in production. What's your process?**
- **Why ask**: Debugging approach
- **Good answer**: Reproduce locally, check logs, git bisect, fix, test, deploy
- **Red flag**: "Fix it directly in production"

**Q2: How do you stay updated with technology?**
- **Why ask**: Learning mindset
- **Good answer**: Documentation, newsletters, side projects, open source
- **Red flag**: "I don't have time"

## Red Flags (Auto-Reject)

- Can't explain REST API basics
- Never used Git
- Copy-pastes without understanding
- Never asks questions
- Defensive about feedback
- Ghosts during interview process

## Green Flags (Strong Candidates)

- Side projects on GitHub
- Open source contributions
- Explains concepts simply
- Asks thoughtful questions
- Handles "I don't know" gracefully
- Writes about what they learn

## Technical Challenge Format

**Time**: 90 minutes  
**Setup**: Live coding session (VS Code Live Share or similar)  
**Rules**:
- Google allowed
- Ask questions
- Explain your thinking
- Tests required

**Backend Challenge**:
```
Build FastAPI API for task management:
- POST /tasks (create)
- GET /tasks (list with filters)
- GET /tasks/{id} (get one)
- PUT /tasks/{id} (update)
- DELETE /tasks/{id} (delete)

Requirements:
- Pydantic validation
- SQLAlchemy models
- Error handling
- 2+ tests
- README with setup

Bonus: Pagination, filtering, async
```

**Frontend Challenge**:
```
Build React task dashboard:
- List tasks from API
- Add new task form (validation)
- Mark complete/incomplete
- Delete task
- Loading states
- Error handling

Requirements:
- TypeScript
- Error boundaries
- Responsive
- 1+ test
- README

Bonus: Optimistic updates, drag-drop
```

**Evaluation Criteria**:
1. Working code (30%)
2. Code organization (25%)
3. Error handling (20%)
4. Tests (15%)
5. Communication (10%)

## Compensation

- **Stipend**: Competitive monthly rate
- **Equipment**: MacBook Pro or equivalent
- **Learning**: $500 course/conference budget
- **Conversion**: Full-time offers for top 20%

## Timeline

**Week 1**: Applications open  
**Week 2-3**: Resume screening + coding challenges  
**Week 4**: Technical interviews  
**Week 5**: Culture fit + offers  
**Week 6**: Onboarding begins

## Apply

Email: himanshu.dixit@nexod.ai

Subject: `[Internship] Backend/Frontend/DevOps - [Your Name]`

Attach:
1. Resume (PDF)
2. GitHub/GitLab link
3. One deployed project
4. Expected start date

**No cover letters. Code speaks.**

---

*Last updated: Feb 2026*
