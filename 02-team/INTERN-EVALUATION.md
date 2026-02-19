# Intern Evaluation Framework

**Propnet.ai - Real Estate Platform**

---

## What We're Building

Real estate platform like Propnet. The hard part isn't listing properties - it's building trust in a high-stakes transaction where mistakes cost people money.

**What makes this platform hard:**

- Properties have 30-50 photos, maps with thousands of pins, real-time status changes
- Search needs to handle 15+ filters plus geo queries
- AI matching "I want a modern house near work but not noisy" is genuinely hard
- Scheduling involves time zones, conflicts, reminders
- This isn't a toy app - people's life savings are involved

---

## What We Look For

**Education**: CS degree preferred, but if you've built things and can explain your code, that's what matters.

**The real test**: Can you figure things out? Can you explain your thinking?

---

## Frontend Intern

### Technical Requirements

#### Must Know

**React**
- useState, useEffect, useCallback, useMemo, useRef
- Custom hooks
- Context API
- How re-rendering works

**The part that trips everyone up**:
> "Why is my component re-rendering infinitely?"

Your useEffect has a function in the dependency array that changes on every render. Fix: wrap in useCallback or move inside useEffect.

**Interview question**: "Walk me through what happens when this useEffect runs."

**TypeScript**
- Basic types, interfaces, types
- Generics at basic level
- Why "any" is bad

**Interview question**: "Type this function: takes array of objects with id, returns object with that id."

**HTTP/APIs**
- GET, POST, PUT, DELETE
- Status codes: 200, 201, 400, 401, 403, 404, 500
- Query params vs body

**Interview question**: "When would you use GET vs POST? What about PUT vs PATCH?"

**CSS**
- Box model
- Flexbox (justify-content vs align-items)
- Grid basics
- Media queries

**Interview question**: "Center a div both horizontally and vertically. Three ways."

#### Should Know

- Next.js App Router basics
- React Query or SWR basics
- Git (commit, push, PR)

#### We'll Ask About Real Estate Stuff

**Maps**
- How would you render 5,000 property markers on a map without it being slow?
- What is marker clustering?

**Real-time**
- Property status changes from "available" to "under contract" - how does the frontend know?
- WebSocket vs polling vs Server-Sent Events

**Forms**
- Property listing form has 30 fields - how do you handle state?
- What happens if user refreshes mid-form?

**Libraries we use**: Next.js 14, React Query, Zustand, Tailwind CSS, React Hook Form + Zod, Playwright

---

## Backend Intern

### Technical Requirements

#### Must Know

**Python**
- Classes, decorators, async/await
- List comprehensions, generators
- Type hints

**The part that trips everyone up**:
> "Why doesn't this async code run in parallel?"

You called await on each function instead of asyncio.gather(). Async is concurrent, not parallel.

**Interview question**: "What's the difference between sync and async? When would you use each?"

**REST APIs**
- What makes something RESTful
- Proper status codes
- Request/response lifecycle

**Interview question**: "Design an endpoint to list properties with filters. What should the URL look like?"

**SQL**
- SELECT, INSERT, UPDATE, DELETE
- JOINs (INNER, LEFT)
- WHERE, GROUP BY, ORDER BY
- Basic indexes

**Interview question**: "Write a query to get all properties with their agent's name."

**FastAPI + Pydantic**
- How validation works
- Dependency injection
- Why we use schemas

**The part that trips everyone up**:
> "Why do I need both SQLAlchemy model and Pydantic schema?"

One is for the database, one is for the API. They're different representations.

**Interview question**: "What's the difference between a SQLAlchemy model and a Pydantic schema?"

#### Should Know

- Virtual environments
- Basic authentication (JWT)
- Git workflow

#### We'll Ask About Real Estate Stuff

**Search**
- Property search with 15 filters - how do you build the query?
- "Properties within 5 miles of this location" - how do you do that?

**Scheduling**
- User wants to schedule a showing - what data do you store?
- Time zones - user in EST schedules showing for agent in PST

**Media**
- Property has 20 photos - how do you handle upload?
- Image processing (thumbnails) - sync or async?

**Libraries we use**: FastAPI, SQLAlchemy 2.0 (async), Alembic, Celery + Redis, Pydantic v2, python-jose, passlib

---

## AI/Agent Intern

### Technical Requirements

#### Must Know

**LLM Basics**
- What is temperature?
- What are tokens?
- System prompt vs user prompt
- Few-shot prompting

**The part that trips everyone up**:
> "Why does the same prompt give different answers?"

Temperature controls randomness. 0 = deterministic, 1 = very creative. LLMs are stochastic.

**Interview question**: "What's the difference between temperature 0 and 0.9? When would you use each?"

**Basic Prompting**
- Writing clear instructions
- Providing examples
- Prompt structure

**Interview question**: "Write a prompt for a real estate agent that suggests properties based on user preferences."

**RAG Basics**
- What are embeddings?
- What is vector search?
- Why would you use Pinecone instead of LIKE query?

**The part that trips everyone up**:
> "Why can't I just use PostgreSQL text search?"

Semantic search vs keyword search. "House" and "home" are different words but similar meaning. Embeddings capture that.

**Interview question**: "How would you implement RAG so an agent can answer questions about our property listings?"

**Agent Concepts**
- Tool use
- Sequential execution
- Error handling

**Interview question**: "What happens if an agent's tool call fails?"

#### Should Know

- Python (obviously)
- Basic API consumption
- JSON handling

#### We'll Ask About Real Estate Stuff

**Property Matching**
- User says "I want something modern but not too far from downtown" - how does the AI translate that to filters?

**Conversations**
- User has a 20-message conversation about their preferences - how does the agent remember everything without hitting context limits?

**Market Analysis**
- "What's a fair price for this house?" - how would you build this?

**Libraries we use**: CrewAI, LangGraph, LangChain, OpenAI SDK, Pinecone, tiktoken

---

## Real Estate Platform - Where We'll Have Trouble

This is what keeps me up at night. When you're interviewing, think about these:

### 1. Maps Performance
We're going to have thousands of properties. Putting all markers on a map will crash the browser.

**What we'll ask**: "How would you render 5,000 property markers on a map?"

**What we're looking for**: Marker clustering, viewport-based loading, virtualization

### 2. Real-Time Updates
Property status changes instanty. If user A views a property and user B buys it, user A should know immediately.

**What we'll ask**: "How does the frontend know when a property status changes?"

**What we're looking for**: WebSocket, SSE, or polling - with trade-offs

### 3. Search Performance
15 filters plus geo queries on 100k+ properties.

**What we'll ask**: "Design a property search endpoint with filters for price, beds, baths, sqft, and location."

**What we're looking for**: Query building, indexing strategy, caching

### 4. Image Handling
Properties have 20-50 photos. Upload, process, serve.

**What we'll ask**: "How do you handle uploading 30 photos for a property listing?"

**What we're looking for**: Async processing, background jobs, CDN

### 5. Agent Reliability
AI agents hallucinate. They get into loops. They timeout.

**What we'll ask**: "What happens when the agent keeps suggesting the same property over and over?"

**What we're looking for**: Error handling, retry logic, guardrails

### 6. Context Windows
Conversations get long. LLM context windows have limits.

**What we'll ask**: "User has 50 messages of conversation history. How do you handle that?"

**What we're looking for**: Summarization, truncation, retrieval

---

## Evaluation Rubric

### Frontend

| Area | Poor (1) | Fair (2) | Good (3) | Excellent (4) |
|------|-----------|-----------|----------|---------------|
| React Fundamentals | Can't explain useState/useEffect | Basic usage | Comfortable | Deep understanding |
| TypeScript | Uses any | Basic types | Proper typing | Advanced patterns |
| CSS | Can't center div | Basic layouts | Flexbox/Grid | Responsive expert |
| Problem Solving | Needs constant help | Some guidance | Independent | Solves novel problems |
| Communication | Silent | Asks after long time | Good questions | Explains thinking |

**Passing**: 12+ / 20

### Backend

| Area | Poor (1) | Fair (2) | Good (3) | Excellent (4) |
|------|-----------|-----------|----------|---------------|
| Python/Async | Sync only | Basic async | Comfortable | Deep understanding |
| APIs | Can't design | Basic CRUD | Proper REST | Advanced patterns |
| Database | Can't write JOIN | Basic queries | Optimization aware | Expert |
| FastAPI/Pydantic | Never used | Basic usage | Validation | Custom patterns |
| Problem Solving | Needs constant help | Some guidance | Independent | Solves novel problems |

**Passing**: 12+ / 20

### AI/Agent

| Area | Poor (1) | Fair (2) | Good (3) | Excellent (4) |
|------|-----------|-----------|----------|---------------|
| LLM Concepts | No understanding | Basic prompting | Temperature, tokens | Advanced |
| RAG/Vectors | Never heard | Basic concept | Can implement | Expert |
| Agent Patterns | No experience | Basic tools | Sequential | Complex |
| Python | Weak | Basic | Comfortable | Strong |
| Problem Solving | Needs constant help | Some guidance | Independent | Solves novel problems |

**Passing**: 12+ / 20

---

## Practical Challenge

### Frontend (90 min)

Build property listing page:
- Display 10 properties from mock API
- Filter by price range (min/max inputs)
- Sort by price, date
- Click property for details modal
- TypeScript
- Basic test

### Backend (90 min)

Build property API:
- GET /properties (list with filters)
- POST /properties (create)
- GET /properties/{id}
- PUT /properties/{id}
- DELETE /properties/{id}
- Pydantic validation
- Basic test

### AI/Agent (90 min)

Build property agent:
- Takes user query ("find me a 3BR under 500k")
- Uses search tool to find properties
- Returns formatted response
- Basic prompt engineering
- Error handling

---

## What We Actually Evaluate

1. **Can you figure it out?** - Not "do you know everything"
2. **Is your code readable?** - Not "is it clever"
3. **Do you handle errors?** - Not "does it work in happy path"
4. **Can you explain it?** - Not "do you have answers"
5. **Do you test?** - Not "is it perfect"

---

## Hardest Topics - What Actually Confuses People

### Frontend

| Topic | Why It's Hard | How to Explain |
|-------|---------------|----------------|
| useEffect dependencies | The array controls when effect runs. Missing something = infinite loop. Too much = performance issues. | Draw the dependency flow. Show example of adding console.log and seeing it run infinitely. |
| Flexbox axis | justify vs align. Main vs cross. Confuses everyone. | Draw the axis. justify-content = main axis, align-items = cross axis. |
| State sync | React Query vs Zustand vs URL. Which to use when? | Server state = React Query. Client state = Zustand. Shareable = URL. |

### Backend

| Topic | Why It's Hard | How to Explain |
|-------|---------------|----------------|
| Async/concurrency | Event loop is invisible. You can't see it. | Show asyncio.gather() vs sequential await. Demonstrate with timing. |
| Query optimization | You can't see what's slow without tools. | Run EXPLAIN ANALYZE. Show difference between indexed and non-indexed. |
| API design | Getting abstraction wrong breaks clients later. | Show examples of bad APIs. Discuss versioning. |

### AI

| Topic | Why It's Hard | How to Explain |
|-------|---------------|----------------|
| LLM randomness | Non-deterministic. Same prompt = different answers. | Show temperature demo. 0 vs 1. Explain "stochastic parrot." |
| RAG quality | Is it the embedding? Chunking? Retrieval? Prompt? | Walk through each step. Show where things can break. |
| Agent loops | Agent keeps doing same thing wrong. | Explain guardrails. Show timeout/retry logic. |

---

## Specific Libraries You'll See in Our Code

### Frontend
- Next.js 14 (App Router, Server Components)
- React Query (server state)
- Zustand (client state)
- Tailwind CSS
- React Hook Form + Zod (validation)
- @tanstack/react-table (data tables)
- Recharts (charts)
- Mapbox GL JS or @react-google-maps/api
- Socket.io-client or native WebSocket

### Backend
- FastAPI
- SQLAlchemy 2.0 (async)
- Alembic (migrations)
- Celery + Redis (background jobs)
- Pydantic v2
- python-jose (JWT)
- passlib + bcrypt
- httpx (async HTTP)
- asyncpg

### AI
- CrewAI
- LangGraph
- LangChain
- OpenAI SDK
- Pinecone
- tiktoken (token counting)
- sentence-transformers (embeddings)

---

## Questions to Ask Candidates

### Frontend
1. "Walk me through how React renders a component"
2. "When would you use React Query vs useState?"
3. "How would you optimize a slow React app?"
4. "What happens when you click this button?"

### Backend
1. "What's the difference between sync and async in Python?"
2. "How would you design an API for filtering 100k properties?"
3. "What happens when a database query times out?"
4. "How would you handle 50 image uploads for one property?"

### AI
1. "How would you improve a prompt that's giving bad answers?"
2. "What's the difference between semantic and keyword search?"
3. "How would you handle an agent that keeps looping?"
4. "User asks 'find me a house like this one' - how does the AI understand 'like this one'?"

---

*Propnet.ai - Building Real Estate's Future*
