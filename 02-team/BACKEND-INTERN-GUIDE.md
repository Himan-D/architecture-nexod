# Backend Intern Hiring Guide

**Role**: Backend Engineering Intern (2-3 positions)
**Duration**: 12 weeks
**Start Date**: Flexible
**Location**: Remote (overlap 10am-4pm EST)

## What You'll Build

Real production code. Not toy projects. You'll:
- Build API endpoints that serve real users
- Design database schemas for multi-tenant SaaS
- Integrate with OpenAI, Stripe, and other third-parties
- Write tests that run in CI/CD
- Deploy to production (yes, really)

## Minimum Requirements

Don't apply if you can't check these:

**Python**:
- Built something with FastAPI, Flask, or Django
- Understand `async`/`await` (can explain the event loop)
- Written classes, used inheritance, understand decorators
- Used list/dict comprehensions
- Handled exceptions properly (try/except, not bare except)

**Databases**:
- Written SQL: JOINs, GROUP BY, subqueries
- Used an ORM (SQLAlchemy, Django ORM, etc.)
- Understand indexes (when to add them)
- Know what ACID means

**Git**:
- Used branches (not just main/master)
- Made pull requests
- Resolved merge conflicts
- Written commit messages that explain "why" not "what"

**HTTP/APIs**:
- Built a REST API
- Know GET vs POST vs PUT vs DELETE
- Understand HTTP status codes (200, 201, 400, 401, 404, 500)
- Used Postman or similar to test APIs

**Terminal**:
- Navigate directories
- Run Python scripts
- Use pip/virtualenv
- SSH into servers (bonus)

## Nice to Have (Not Required)

- Docker (built/ran containers)
- Redis (used as cache or message broker)
- pytest (written unit tests)
- AWS (EC2, S3 basics)
- Celery (background jobs)
- OpenAPI/Swagger documentation

## Stack You'll Use

```
FastAPI 0.104
SQLAlchemy 2.0 (async)
Alembic (migrations)
PostgreSQL 15
Redis 7
Celery 5
pytest
Pydantic v2
httpx (async HTTP)
sentry-sdk
prometheus-client
structlog (JSON logging)
```

## What We Expect

**Week 1-2**: Get productive
- Local dev environment running
- First PR merged (fix a bug, add a test)
- Understand our codebase structure
- Ask questions (but try for 30 mins first)

**Week 3-6**: Build features
- Own a feature end-to-end
- Write tests (we require 80%+ coverage on new code)
- Participate in code reviews
- Deploy to staging

**Week 7-10**: Complex work
- Async processing with Celery
- AI integration (OpenAI API)
- Performance optimization
- Database query tuning

**Week 11-12**: Ship to production
- Complete a significant feature
- Handle edge cases, error states
- Write documentation
- Present your work to the team

## Interview Process

**Round 1: Coding Challenge (90 mins)**
Build a simple FastAPI API with:
- 3-4 CRUD endpoints
- Pydantic validation
- SQLAlchemy models
- One test

We care about:
- Code organization
- Error handling
- Type hints
- Tests (even basic ones)
- README with setup instructions

**Round 2: Technical Interview (45 mins)**
- Review your challenge code
- System design question (design a URL shortener)
- Python/FastAPI questions
- Database questions

**Round 3: Culture Fit (30 mins)**
- Tell us about a project you built
- How do you handle being stuck?
- Questions for us

## Red Flags (We Won't Hire You If)

- Can't explain what a REST API is in simple terms
- Never used Git (we don't care about GUI vs CLI, but you need version control)
- Copy-pastes code without understanding it
- Doesn't ask any questions
- Can't write a SQL JOIN
- Gets defensive about feedback

## Green Flags (We Really Want You If)

- Has side projects on GitHub (even incomplete ones)
- Contributed to open source
- Built something with our stack before
- Explains technical concepts clearly
- Handles feedback gracefully
- Writes about what they learned (blog, notes, etc.)

## Compensation

- **Stipend**: $X,XXX/month (competitive for interns)
- **Equipment**: We'll send you a laptop if needed
- **Learning**: Conference budget, courses, books
- **Conversion**: Strong performers get full-time offers

## Apply

Send to: engineering-jobs@company.com

Include:
1. Resume (PDF)
2. Link to GitHub/GitLab
3. Link to one project you're proud of (with README)
4. 2-3 sentences about why this role

**No cover letters. We won't read them.**

---

## Technical Interview Questions (For Reference)

We'll ask 3-4 of these:

1. **What's the difference between `async def` and `def` in FastAPI? When would you use each?**

2. **You have a User model and a Post model. Write the SQLAlchemy relationship.**
   ```python
   # Expected answer structure:
   class User(Base):
       posts = relationship("Post", back_populates="author")
   
   class Post(Base):
       author_id = Column(ForeignKey("users.id"))
       author = relationship("User", back_populates="posts")
   ```

3. **What's an Alembic migration? Why use it instead of manually running SQL?**

4. **Your API is slow. How do you debug it?** (Looking for: logging, profiling, database query analysis)

5. **Explain dependency injection in FastAPI. Give an example.**

6. **How do you handle database transactions in SQLAlchemy? What happens if an error occurs?**

7. **What's the difference between 401 and 403 HTTP status codes?**

8. **You need to send 10,000 emails. How would you architect this?** (Looking for: background jobs, queue, retries)

---

## What Success Looks Like

**Week 6**: Can build a feature independently, writes tests, handles review feedback without multiple rounds

**Week 12**: Ships production code, mentors newer interns, proposes architecture improvements

---

*Last updated: Feb 2026*
