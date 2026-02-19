# Intern Evaluation Framework

**For**: Frontend, Backend, AI Engineers  
**Platform**: Real Estate Agent Platform (Uber-like)  
**Owner**: Himanshu Dixit

---

## What We're Building

Think Uber for real estate. Buyers, sellers, agents all on one platform. AI agents handle the heavy lifting - scheduling, matching, follow-ups, market analysis.

The tricky part: Real estate is relationship-driven. Agents need trust. Our platform needs to feel personal even though it's AI-assisted.

---

## Education Baseline

### Minimum

- **Computer Science** (preferred) or related field
- Or self-taught with demonstrated projects
- Or bootcamp graduate with strong portfolio

**What we actually care about**:
- Can you code? That's it.
- Degrees are signals, not gatekeepers.
- Show me projects, not diplomas.

---

## Concept Understanding by Role

---

### Frontend Intern

#### Must Understand

**1. React Fundamentals**
- How rendering works (virtual DOM, reconciliation)
- State vs props
- useEffect cleanup (this trips people up)
- Why components re-render

**Hardest to understand**: 
> "Why is my component re-rendering infinitely?"
> 
> Understanding the dependency array in useEffect is the hardest part. Most interns miss that functions in the array need to be stable (wrapped in useCallback) or they'll cause infinite loops.

**How to test**: Ask them to debug a component that's re-rendering in a loop.

**2. TypeScript Basics**
- Basic types, interfaces
- Generics (at least simple ones)
- Why "any" is bad

**Hardest to understand**:
> "Why does this generic type work?"
> 
> Generics are abstract. Most get stuck on `<T>` syntax without understanding the principle of "this thing can be any type, but once chosen, it's consistent."

**How to test**: Ask them to type a simple function that takes an array and returns the first item.

**3. HTTP/API Basics**
- GET vs POST vs PUT vs DELETE
- Status codes (200, 201, 400, 401, 404, 500)
- How to read API responses

**Hardest to understand**:
> "Why is my POST request failing?"
> 
> Understanding that POST creates, PUT replaces, PATCH modifies. And that 400 usually means "you sent something wrong" while 401 means "who are you?"

**4. CSS Fundamentals**
- Box model
- Flexbox (the justify-content vs align-items confusion)
- Responsive basics (media queries)

**Hardest to understand**:
> "Why isn't my flexbox centering working?"
> 
> justify-content is main axis, align-items is cross axis. Confuses everyone initially.

#### Should Know

- Next.js basics (pages vs app router - we use app router)
- Basic testing (render, screen from testing library)
- Git basics (commit, push, PR)

#### Nice to Have (Bonus)

- React Query or similar
- Zustand or Redux
- Playwright basics

---

### Backend Intern

#### Must Understand

**1. Python Async**
- Difference between `def` and `async def`
- What `await` actually does
- Why async matters for I/O

**Hardest to understand**:
> "Why does my async function not run in parallel?"
> 
> Understanding the event loop. Many think async = parallel. It's concurrent, not parallel. You need to use asyncio.gather() or similar to run things concurrently.

**How to test**: Ask them to explain what happens when you call two async functions.

**2. REST APIs**
- What makes something RESTful
- Proper status codes
- Request/response lifecycle

**Hardest to understand**:
> "Should this be GET or POST?"
> 
> GET = read (no side effects), POST = create, PUT = replace, DELETE = remove. The hardest part is knowing when to use query params vs body.

**3. SQL Basics**
- SELECT, INSERT, UPDATE, DELETE
- JOINs (INNER, LEFT)
- Basic WHERE clauses

**Hardest to understand**:
> "Why is this query slow?"
> 
> Understanding indexes and query execution. Most don't realize that SELECT * is usually bad, or that missing indexes make queries scan entire tables.

**How to test**: Ask them to write a query joining users and their properties.

**4. FastAPI/Pydantic**
- How validation works
- Dependency injection
- Why we use schemas

**Hardest to understand**:
> "Why do I need both a model and a schema?"
> 
> SQLAlchemy model = database representation. Pydantic schema = API representation. They're different. This confuses everyone.

#### Should Know

- Basic Python (classes, decorators, typing)
- Git workflow
- Virtual environments

#### Nice to Have

- Celery basics
- Redis concepts
- Basic authentication (JWT)

---

### AI/Agent Engineer Intern

#### Must Understand

**1. How LLMs Work (Conceptually)**
- Not the math, but what temperature, tokens, prompts do
- Why "prompt engineering" matters

**Hardest to understand**:
> "Why does the same prompt give different answers?"
> 
> Temperature = randomness. Higher = more creative, less consistent. Most don't understand that LLMs are stochastic, not deterministic.

**2. Basic Prompting**
- System vs user prompts
- Few-shot examples
- Why context matters

**Hardest to understand**:
> "Why is the model ignoring my instructions?"
> 
> Prompt structure matters. System prompt sets behavior, user prompt provides input. But the model注意力 (attention) mechanism means earlier tokens sometimes get "forgotten" in long prompts.

**3. What RAG Is**
- Embeddings (at concept level)
- Vector search
- Why we'd use this

**Hardest to understand**:
> "Why do I need Pinecone? I can just search text."
> 
> Semantic search vs keyword search. "House" and "home" are different words but similar meaning. Embeddings capture meaning, not just matches.

**4. Agent Concepts**
- Tool use
- Sequential vs parallel agent execution
- What can go wrong (hallucination, loops)

**Hardest to understand**:
> "Why is the agent doing X when I asked for Y?"
> 
> Agents can get into weird loops or misinterpret instructions. Understanding that you need to constrain behavior through prompts and tools.

#### Should Know

- Python (obviously)
- Basic API consumption
- JSON handling

#### Nice to Have

- LangChain/CrewAI basics
- Vector database concepts

---

## Real Estate Platform Context

This isn't a simple CRUD app. Here's what makes it hard:

### For Frontend

**1. Real-time Updates**
- Property status changes (available → under contract → sold)
- Agent availability
- Chat/messaging

**Why it's hard**: WebSocket vs polling. When to use which. Managing connection state.

**2. Maps and Location**
- Property markers on map
- Neighborhood boundaries
- Distance calculations

**Why it's hard**: Google Maps API integration, marker clustering, performance with many pins.

**3. Complex Forms**
- Property listings have 20+ fields
- Image uploads with cropping
- Multi-step wizards

**Why it's hard**: State management across steps. Validation. Performance.

### For Backend

**1. Search & Filtering**
- Filters on 15+ fields
- Geo queries (within 5 miles of X)
- Sorting by price, date, relevance

**Why it's hard**: Database query optimization. When to filter in DB vs memory.

**2. Real-time Communication**
- WebSocket connections
- Message delivery
- Online/offline status

**Why it's hard**: Connection management. Scale. Reconnection logic.

**3. Scheduled Tasks**
- Open house reminders
- Follow-up automation
- Price drop alerts

**Why it's hard**: Cron jobs that actually run. Time zones. Missing notifications.

### For AI Agents

**1. Matching Buyers to Properties**
- Preference learning
- Smart recommendations
- Explainability ("why this property?")

**Why it's hard**: It's not just filtering. It's understanding "I want something modern but not too far from work."

**2. Conversation Context**
- Remembering chat history
- Not repeating questions
- Handling interruptions

**Why it's hard**: LLM context windows are limited. Need to summarize or truncate history.

**3. Agent Reliability**
- What happens when API fails
- Timeout handling
- Graceful degradation

**Why it's hard**: Agents can hallucinate or loop. Need guardrails.

---

## Evaluation Rubric

### Frontend Intern

| Category | Poor (1) | Fair (2) | Good (3) | Excellent (4) |
|----------|----------|-----------|----------|---------------|
| **React Fundamentals** | Can't explain state vs props | Understands basics but struggles with hooks | Comfortable with hooks, can debug | Deep understanding, can explain why |
| **TypeScript** | Avoids types, uses any | Basic types, some generics | Proper typing, avoids any | Advanced types, generics |
| **CSS/Layout** | Struggles with basics | Can do simple layouts | Flexbox/Grid confident | Responsive, cross-browser |
| **Problem Solving** | Needs lots of help | Some guidance needed | Independent for standard issues | Can debug complex issues |
| **Communication** | Doesn't ask questions | Asks after too long | Asks good questions | Explains their thinking |

**Passing Score**: 12+ out of 20

### Backend Intern

| Category | Poor (1) | Fair (2) | Good (3) | Excellent (4) |
|----------|----------|-----------|----------|---------------|
| **Python/Async** | Sync only | Basic async, no await | Comfortable with async | Deep event loop understanding |
| **APIs** | Doesn't understand REST | Basic CRUD | Proper design, status codes | Advanced patterns |
| **Database** | Can't write JOIN | Basic queries | Optimization awareness | Can explain query plans |
| **FastAPI/Pydantic** | Never used | Basic usage | Validation, DI | Custom validators, patterns |
| **Problem Solving** | Needs lots of help | Some guidance needed | Independent for standard issues | Can debug complex issues |

**Passing Score**: 12+ out of 20

### AI/Agent Intern

| Category | Poor (1) | Fair (2) | Good (3) | Excellent (4) |
|----------|----------|-----------|----------|---------------|
| **LLM Concepts** | No understanding | Basic prompting | Temperature, tokens, context | Advanced techniques |
| **RAG/Vectors** | Never heard | Basic concept | Can implement simple RAG | Vector optimization |
| **Agent Patterns** | No experience | Basic tool use | Sequential agents | Complex orchestration |
| **Python** | Weak | Basic | Comfortable | Strong |
| **Problem Solving** | Needs lots of help | Some guidance needed | Independent | Can debug complex issues |

**Passing Score**: 12+ out of 20

---

## Practical Evaluation

### Coding Challenge Format

**Frontend** (90 min):
Build a property listing page with:
- Display 10 properties from mock API
- Filter by price range (slider)
- Sort by price/date
- Click property to see details modal
- TypeScript required
- Basic tests

**Backend** (90 min):
Build API for properties:
- GET /properties (list with filters)
- POST /properties (create)
- GET /properties/{id} (detail)
- PUT /properties/{id} (update)
- DELETE /properties/{id}
- Pydantic validation
- Basic tests

**AI/Agent** (90 min):
Build a simple agent that:
- Takes a user query about real estate
- Uses search tool to find properties
- Returns formatted response
- Basic prompt engineering
- Error handling

### What We're Actually Evaluating

1. **Can you figure it out?** (Problem solving)
2. **Is your code readable?** (Code quality)
3. **Do you handle errors?** (Edge cases)
4. **Can you explain it?** (Communication)
5. **Do you test?** (Quality mindset)

Not:
- Whether you know every syntax
- Whether you finish everything
- Whether you use "perfect" solution

---

## What Makes Someone Successful Here

**The internal drive to understand why, not just how.**

- "It works" → "Why does it work?"
- "I used useState" → "When should I use useState vs useReducer?"
- "The API failed" → "Why did it fail? How do I handle it?"

**Asking questions at the right time.**

- Stuck for 30+ minutes? Ask.
- Not sure if there's a better approach? Ask.
- Don't understand a requirement? Ask.

**Taking ownership.**

- If it's your task, it's your problem to solve.
- Doesn't mean you have to solve it alone, but you have to drive it.

---

## Questions to Ask

### Frontend
1. "Walk me through how React renders a component"
2. "When would you use Context vs a state management library?"
3. "How would you optimize a slow React app?"

### Backend
1. "What's the difference between sync and async in Python?"
2. "How would you design an API for filtering 1M properties?"
3. "What happens when a database query times out?"

### AI/Agent
1. "How would you improve a prompt that's giving bad answers?"
2. "What's the difference between semantic and keyword search?"
3. "How would you handle an agent that keeps looping?"

---

## Hardest Topics Summary

| Role | Hardest Topic | Why |
|------|---------------|-----|
| Frontend | useEffect dependencies | Infinite loops, stale closures |
| Frontend | Flexbox | Axis confusion |
| Backend | Async/concurrency | Event loop is abstract |
| Backend | Query optimization | Can't see what's slow without tools |
| Backend | API design | Knowing the right abstractions |
| AI | LLM randomness | Hard to debug non-deterministic code |
| AI | RAG at scale | Embedding management, retrieval quality |
| AI | Agent reliability | Hallucination, loops, timeouts |

---

## Apply

**Email**: himanshu.dixit@nexod.ai

Include:
1. GitHub
2. One deployed project
3. Brief explanation of what you built and why

No cover letter. Code speaks.

---

himanshu.dixit@nexod.ai
