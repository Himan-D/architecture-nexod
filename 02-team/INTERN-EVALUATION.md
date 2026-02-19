# Intern Evaluation Framework

**Propnet.ai - AI-Powered Real Estate Platform**

---

## What We're Building

Propnet.ai is transforming real estate. It's Uber for real estate - buyers, sellers, agents, all connected through AI.

The platform handles:
- Property listings and searches
- AI-powered property matching
- Scheduling showings and open houses
- Market analysis and price predictions
- Agent-client communication

This isn't a simple CRUD app. It's real-time, data-heavy, and trust-critical.

---

## Education Requirements

### What We Look For

**Preferred**:
- Computer Science or related field
- Strong fundamentals in data structures, algorithms

**Acceptable**:
- Self-taught with strong portfolio
- Bootcamp with demonstrated projects
- Related technical field

**What Actually Matters**:
- Can you code? That's it.
- Projects speak louder than diplomas.

---

## Frontend Intern

### Must Understand

#### 1. React Fundamentals
**What it means**: You build UIs. You need to know how React works under the hood.

**The hard part**:
> "Why is my component re-rendering infinitely?"

Most interns struggle with useEffect dependencies. The dependency array tells React when to re-run. Missing something = infinite loop. Adding everything = performance issues.

**How to evaluate**: Ask them to debug a component that's re-rendering infinitely.

**What good looks like**: They can explain why useEffect runs, when it runs, and how to prevent infinite loops.

#### 2. TypeScript Basics
**What it means**: Code should be safe. Types catch bugs before production.

**The hard part**:
> "Why does this generic work?"

Generics are abstract. `<T>` means "this can be any type." Understanding that once chosen, the type stays consistent is the key insight.

**How to evaluate**: Ask them to type a function that takes an array and returns the first item.

**What good looks like**: They use proper types, avoid `any`, understand interfaces vs types.

#### 3. HTTP/APIs
**What it means**: Your frontend talks to servers. You need to know how.

**The hard part**:
> "Why is my POST failing?"

Understanding that GET = read (no side effects), POST = create, PUT = replace. And that 400 means "you sent something wrong" while 401 means "who are you?"

**What good looks like**: They know status codes, can read API responses, understand query params vs body.

#### 4. CSS Layout
**What it means**: Things need to look right on all screens.

**The hard part**:
> "Why isn't my flexbox centering?"

justify-content = main axis. align-items = cross axis. This confuses everyone at first.

**What good looks like**: They can build responsive layouts without fighting CSS.

### Should Know
- Next.js basics (we use App Router)
- Basic testing (render, screen)
- Git workflow

### Nice to Have
- React Query
- Zustand
- Playwright

---

## Backend Intern

### Must Understand

#### 1. Python Async
**What it means**: Programs need to handle many things at once.

**The hard part**:
> "Why doesn't my async run in parallel?"

Async = concurrent, not parallel. You need `asyncio.gather()` to run things at the same time. The event loop is confusing at first.

**How to evaluate**: Ask them to explain what happens when you call two async functions.

**What good looks like**: They understand event loop, know when to use await vs gather.

#### 2. REST APIs
**What it means**: Servers expose functionality through HTTP.

**The hard part**:
> "GET or POST?"

GET = read, POST = create, PUT = replace, DELETE = remove. The hardest part is knowing when to use query params vs body.

**What good looks like**: They design proper REST endpoints, use correct status codes.

#### 3. SQL Basics
**What it means**: Data lives in databases. You need to get it out.

**The hard part**:
> "Why is this query slow?"

Understanding indexes and execution plans. Most don't realize that SELECT * is usually bad, or that missing indexes scan entire tables.

**How to evaluate**: Ask them to write a query joining users and their properties.

**What good looks like**: They can write JOINs, understand basic optimization.

#### 4. FastAPI and Pydantic
**What it means**: We validate data and build APIs.

**The hard part**:
> "Why do I need both a model and a schema?"

SQLAlchemy model = database. Pydantic schema = API. They're different layers. This confuses everyone.

**What good looks like**: They use both correctly, understand validation.

---

## AI/Agent Intern

### Must Understand

#### 1. How LLMs Work (Conceptually)
**What it means**: You don't need the math, but you need the concepts.

**The hard part**:
> "Why does the same prompt give different answers?"

Temperature = randomness. Higher = more creative, less consistent. LLMs are stochastic, not deterministic.

**What good looks like**: They understand temperature, tokens, prompts.

#### 2. Basic Prompting
**What it means**: How you ask matters as much as what you ask.

**The hard part**:
> "Why is the model ignoring my instructions?"

Prompt structure matters. System prompt sets behavior, user prompt provides input. But attention mechanism means earlier tokens sometimes get forgotten.

**What good looks like**: They can write effective prompts with examples.

#### 3. What RAG Is
**What it means**: Agents need knowledge beyond training.

**The hard part**:
> "Why do I need Pinecone? I can just search text."

Semantic search vs keyword. "House" and "home" are different words but similar meaning. Embeddings capture meaning.

**What good looks like**: They understand embeddings, vector search, chunking.

#### 4. Agent Concepts
**What it means**: AI that takes actions, not just answers.

**The hard part**:
> "Why is the agent doing X when I asked for Y?"

Agents can misinterpret, loop, or hallucinate. Need guardrails.

**What good looks like**: They understand tool use, error handling, limits.

---

## Real Estate Platform Context

Propnet.ai challenges that will affect your work:

### Frontend Challenges

**1. Real-Time Updates**
- Property status changes (available → under contract → sold)
- Multiple users viewing same property
- Chat messages

**Why it's hard**: WebSocket management. Connection state. Reconnection logic.

**2. Maps Integration**
- Property markers on map
- Neighborhood boundaries
- Distance calculations

**Why it's hard**: Performance with many markers. Different zoom levels.

**3. Complex Forms**
- Property listings (30+ fields)
- Image uploads
- Multi-step wizards

**Why it's hard**: State across steps. Validation. Performance.

### Backend Challenges

**1. Search & Filtering**
- 15+ filter options
- Geo queries (within 5 miles)
- Sorting

**Why it's hard**: Query optimization. Indexing strategies.

**2. Real-Time Communication**
- WebSocket connections
- Message delivery
- Online/offline

**Why it's hard**: Connection management. Scale.

**3. Scheduled Tasks**
- Open house reminders
- Follow-up automation
- Price alerts

**Why it's hard**: Cron reliability. Time zones.

### AI Challenges

**1. Property Matching**
- Preference learning
- "I want something modern but not noisy"
- Explainability

**Why it's hard**: Preferences are complex. Attribution is hard.

**2. Conversation Context**
- Remembering chat history
- Not repeating questions

**Why it's hard**: Context windows are limited.

**3. Market Analysis**
- Price predictions
- Market trends
- Comparable analysis

**Why it's hard**: Real estate is local. Data can be sparse.

---

## Evaluation Rubric

### Frontend Intern

| Category | Poor (1) | Fair (2) | Good (3) | Excellent (4) |
|----------|----------|-----------|----------|---------------|
| React Fundamentals | Can't explain state/props | Basic hooks, struggles debugging | Comfortable debugging | Deep understanding |
| TypeScript | Uses any everywhere | Basic types | Proper typing | Advanced types |
| CSS/Layout | Struggles basics | Simple layouts | Flexbox/Grid confident | Responsive expert |
| Problem Solving | Needs lots of help | Some guidance | Independent | Solves complex |
| Communication | Doesn't ask | Asks after long | Good questions | Explains thinking |

**Passing**: 12+ / 20

### Backend Intern

| Category | Poor (1) | Fair (2) | Good (3) | Excellent (4) |
|----------|----------|-----------|----------|---------------|
| Python/Async | Sync only | Basic async | Comfortable | Deep understanding |
| APIs | No REST understanding | Basic CRUD | Proper design | Advanced patterns |
| Database | Can't JOIN | Basic queries | Optimization aware | Query expert |
| FastAPI/Pydantic | Never used | Basic usage | Validation, DI | Custom patterns |
| Problem Solving | Needs lots of help | Some guidance | Independent | Solves complex |

**Passing**: 12+ / 20

### AI/Agent Intern

| Category | Poor (1) | Fair (2) | Good (3) | Excellent (4) |
|----------|----------|-----------|----------|---------------|
| LLM Concepts | No understanding | Basic prompting | Temperature, tokens | Advanced techniques |
| RAG/Vectors | Never heard | Basic concept | Simple RAG | Vector optimization |
| Agent Patterns | No experience | Basic tools | Sequential | Complex orchestration |
| Python | Weak | Basic | Comfortable | Strong |
| Problem Solving | Needs lots of help | Some guidance | Independent | Solves complex |

**Passing**: 12+ / 20

---

## Practical Challenge

### Frontend (90 minutes)

Build a property listing page:
- Display 10 properties from mock API
- Filter by price range (slider)
- Sort by price/date
- Click property for details modal
- TypeScript required
- Basic tests

### Backend (90 minutes)

Build property API:
- GET /properties (list with filters)
- POST /properties (create)
- GET /properties/{id}
- PUT /properties/{id}
- DELETE /properties/{id}
- Pydantic validation
- Basic tests

### AI/Agent (90 minutes)

Build a property agent:
- Takes user query about real estate
- Uses search tool to find properties
- Returns formatted response
- Basic prompt engineering
- Error handling

---

## What We're Evaluating

1. **Problem solving** - Can you figure it out?
2. **Code quality** - Is it readable?
3. **Error handling** - Do you think about edge cases?
4. **Communication** - Can you explain it?
5. **Testing** - Do you care about quality?

Not:
- Knowing every syntax
- Finishing everything
- Perfect solution

---

## What Makes Someone Succeed

**The drive to understand why, not just how.**

- "It works" → "Why does it work?"
- "I used useState" → "When should I use useReducer?"
- "API failed" → "Why? How do I handle it?"

**Asking questions at the right time.**

- Stuck 30+ minutes? Ask.
- Not sure? Ask.
- Don't understand? Ask.

**Taking ownership.**

- Your task = your problem to solve.
- Drive it, don't just do it.

---

## Hardest Topics Summary

| Role | Hardest Topic | Why |
|------|---------------|-----|
| Frontend | useEffect dependencies | Infinite loops, stale closures |
| Frontend | Flexbox | Axis confusion |
| Backend | Async/concurrency | Event loop is abstract |
| Backend | Query optimization | Can't see without tools |
| Backend | API design | Knowing abstractions |
| AI | LLM randomness | Non-deterministic debugging |
| AI | RAG at scale | Embedding/retrieval issues |
| AI | Agent reliability | Hallucination, loops |

---

## Apply

**Email**: himanshu.dixit@nexod.ai

Include:
1. GitHub profile
2. One deployed project
3. Brief: What did you build and why?

No cover letter. Code speaks.

---

*Propnet.ai - Building the Future of Real Estate*
