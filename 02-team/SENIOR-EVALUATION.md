# Senior Engineer Evaluation Framework

**Propnet.ai - Real Estate Platform**

---

## What We're Building

Real estate platform like Propnet. Not a CRUD app. This is:

- **Real-time**: Property status changes instantly. Multiple people viewing same property need to see updates.
- **Data-heavy**: 100k+ properties, 50 photos each, thousands of markers on a map.
- **AI-intensive**: Agents that match buyers to properties, schedule showings, handle negotiations.
- **Trust-critical**: People's life savings. Mistakes cost money.

---

## Senior vs Intern

**Intern**: Can ship defined tasks  
**Senior**: Defines what to build and how

**Intern**: Asks "how do I?"  
**Senior**: Asks "should we?"

**Intern**: Follows patterns  
**Senior**: Creates patterns

---

## Senior Frontend Engineer

### What We Need

#### 1. Large App Architecture
You've managed 10k+ line codebases. You know when to split, when to combine.

**The hard part**:
> "When do I split this component vs keep it together?"

Co-location is about what loads together, what can be cached, what affects TTI.

**Question**: "Walk me through how you'd structure a dashboard with 50 components."

#### 2. State Management at Scale
Server state, client state, URL state - keeping them in sync is hard.

**The hard part**:
> "Why is my state out of sync?"

React Query for server. Zustand for client. URL for shareable. Knowing when to use which is harder than using them.

**Question**: "User filters properties, shares URL with friend. Friend sees filtered results. How?"

#### 3. Performance
You've taken slow apps and made them fast. You know profiling tools.

**The hard part**:
> "It's slow but I don't know why."

Performance profiling is its own skill. Chrome DevTools isn't enough.

**Question**: "LCP is 4s. How do you find what's causing it?"

### Propnet Challenges

**Maps** - 5,000 property markers
- Marker clustering
- Viewport-based loading
- Different zoom levels = different detail

**Real-time** - Property status changes
- WebSocket management
- Optimistic updates
- Conflict resolution

**Media** - 20-50 photos per property
- Lazy loading
- Image optimization
- Upload queuing

**Forms** - 30+ field property listings
- Multi-step wizards
- Draft saving
- Validation

### Libraries We Use
- Next.js 14 (App Router, Server Components, streaming)
- React Query (TanStack Query v5)
- Zustand
- Tailwind CSS
- React Hook Form + Zod
- @tanstack/react-table
- Recharts or Chart.js
- Mapbox GL JS or @react-google-maps/api
- Socket.io-client
- date-fns

---

## Senior Backend Engineer

### What We Need

#### 1. Database Design
You can design schemas, write queries, optimize performance.

**The hard part**:
> "This query is slow but I don't know why."

EXPLAIN ANALYZE shows you the plan. ORM generates queries, not magic.

**Question**: "Design schema for properties, agents, and showings. How do you find showings for an agent today?"

#### 2. API Design
You've designed APIs used by others. You know versioning.

**The hard part**:
> "Should this be a new endpoint or query param?"

Getting abstraction wrong breaks clients. Breaking changes are expensive.

**Question**: "Design API for property search with 15 filters, pagination, geo queries."

#### 3. Distributed Systems
Caching, queues, eventual consistency.

**The hard part**:
> "Why did the user see old data?"

Caching means stale data. Knowing when consistency matters vs when eventual is fine.

**Question**: "Property status cached in Redis. Agent updates it. How long until all users see new status?"

#### 4. Security
Auth flows, authorization, API security.

**The hard part**:
> "Is this secure?"

Security is a mindset, not a feature.

**Question**: "User can only see properties in their region. How do you enforce this?"

### Propnet Challenges

**Search** - 100k+ properties, 15 filters, geo
- Indexing strategy (GIN, BRIN, spatial)
- Query building
- Caching

**Media** - Image upload/processing
- Async processing (Celery)
- CDN integration
- Thumbnail generation

**Scheduling** - Showings, open houses
- Conflict detection
- Time zone handling
- Reminder system

**Real-time** - WebSocket server
- Connection management
- Room-based messaging
- Reconnection logic

### Libraries We Use
- FastAPI
- SQLAlchemy 2.0 (async)
- Alembic
- Celery + Redis
- Pydantic v2
- python-jose (JWT)
- passlib + bcrypt
- httpx
- asyncpg
- psycopg2-binary

---

## Senior AI/ML Engineer

### What We Need

#### 1. LLM Integration
You've shipped LLM integrations to production.

**The hard part**:
> "The model keeps giving bad answers."

Prompt debugging is hard because you can't step through it.

**Question**: "User complains the agent suggests wrong properties. How do you debug?"

#### 2. Agent Architecture
You've built agent systems that run in production.

**The hard part**:
> "The agent keeps doing X when I asked for Y."

Agents can loop, hallucinate, misinterpret. Guardrails and monitoring are critical.

**Question**: "Agent suggests the same property 10 times. What happened?"

#### 3. Vector Databases
You've implemented RAG systems.

**The hard part**:
> "RAG isn't returning good results."

Is it the embedding? Chunking? Retrieval? Prompt? Need systematic debugging.

**Question**: "User asks 'find homes like this one' - how does this work?"

### Propnet Challenges

**Property Matching**
- "I want modern but not noisy"
- Learning from behavior
- "Why this property?" (explainability)

**Conversation Context**
- 50 message history
- Context window limits
- Summarization strategy

**Market Analysis**
- Price predictions
- Trend analysis
- Comparable sales

**Content Generation**
- Property descriptions
- Marketing copy
- Must sound natural

### Libraries We Use
- CrewAI
- LangGraph
- LangChain
- OpenAI SDK (GPT-4, GPT-4 Turbo)
- Pinecone
- tiktoken
- sentence-transformers (text-embedding-3)
- Weaviate or Chroma (optional)

---

## Where We'll Have Trouble

### 1. Map Performance
5,000 markers crashes browsers.

**Question**: "How do you render 5,000 property markers?"

**Answer we're looking for**: Clustering, viewport loading, virtualization, canvas rendering

### 2. Search Latency
100k properties, 15 filters, geo queries.

**Question**: "How do you make search under 200ms?"

**Answer we're looking for**: Indexing, query optimization, caching, read replicas

### 3. Real-Time Updates
Multiple users viewing same property.

**Question**: "User A and B both viewing property. A buys it. How does B know?"

**Answer we're looking for**: WebSocket, SSE, or polling with trade-offs explained

### 4. Image Processing
30 photos per property, thumbnail generation.

**Question**: "User uploads 30 photos. What happens?"

**Answer we're looking for**: Background job, CDN, async processing, queue

### 5. Agent Reliability
AI keeps making same mistakes.

**Question**: "Agent suggests sold properties. Fix it."

**Answer we're looking for**: Guardrails, validation, retry limits, human review

### 6. Context Management
Long conversations hit LLM limits.

**Question**: "User has 100 messages. What do you do?"

**Answer we're looking for**: Summarization, retrieval, truncation with trade-offs

---

## Evaluation Rubric

### Frontend

| Category | Competent (3) | Strong (4) | Expert (5) |
|----------|---------------|------------|------------|
| Architecture | Can design features | Can design systems | Can design platform |
| Performance | Optimize known issues | Find unknown issues | Prevent issues |
| State | Uses patterns | Chooses right pattern | Creates patterns |
| Code Quality | Clean, tested | Documented, reviewed | Enables others |
| Leadership | Ships features | Mentors interns | Sets direction |

**Minimum**: 15+ / 25

### Backend

| Category | Competent (3) | Strong (4) | Expert (5) |
|----------|---------------|------------|------------|
| Database | Can write queries | Can optimize | Can design schema |
| API Design | RESTful APIs | Versioning, docs | Platform thinking |
| Distributed | Uses caching | Chooses approach | Creates patterns |
| Security | Follows best practices | Audits code | Threat modeling |
| Leadership | Ships features | Mentors interns | Sets direction |

**Minimum**: 15+ / 25

### AI

| Category | Competent (3) | Strong (4) | Expert (5) |
|----------|---------------|------------|------------|
| LLM Integration | API calls work | Prompt optimization | Research-level |
| Agent Systems | Simple agents | Complex orchestration | Production-grade |
| RAG/Vectors | Basic implementation | Evaluation + tuning | Novel approaches |
| ML Concepts | Basic statistics | Modeling | Research |
| Leadership | Ships features | Sets AI strategy | Company direction |

**Minimum**: 15+ / 25

---

## Deep Technical Questions

### Frontend

1. **"Walk me through Next.js 14 App Router. What's different from Pages Router?"**
   - Server Components vs Client Components
   - Streaming, suspense
   - Layouts

2. **"How would you optimize a page with 50 images?"**
   - Lazy loading (native + Intersection Observer)
   - srcset for responsive images
   - CDN
   - Blur placeholder

3. **"Design state for a property filter with 15 options that persists in URL."**
   - URLSearchParams
   - Debouncing
   - Shareable links

4. **"WebSocket disconnects. What do you do?"**
   - Reconnection logic with exponential backoff
   - Queueing messages offline
   - State resync

### Backend

1. **"Design property search for 100k properties with filters and geo."**
   - PostgreSQL with PostGIS
   - Indexing strategy
   - Query building
   - Caching layer

2. **"How do you handle image upload for 30 photos?"**
   - S3 presigned URL
   - Async processing (Celery)
   - CDN invalidation

3. **"Property status cached in Redis. Agent updates. How long until users see it?"**
   - Cache TTL
   - Write-through vs write-back
   - Event-based invalidation

4. **"Design API rate limiting."**
   - Per-user vs per-IP
   - Token bucket vs sliding window
   - Redis implementation

### AI

1. **"Design RAG for property Q&A."**
   - Chunking strategy
   - Embedding model
   - Retrieval
   - Prompt

2. **"Agent keeps hallucinating property prices."**
   - Tool validation
   - Result checking
   - Fallback responses

3. **"User conversation is 10k tokens. What do you do?"**
   - Summarization
   - Retrieval from history
   - Truncation

4. **"How do you measure RAG quality?"**
   - Retrieval metrics (precision, recall)
   - Answer quality (LLM as judge)
   - End-to-end evaluation

---

## Hardest Topics

| Role | Topic | Why It's Hard |
|------|-------|---------------|
| Frontend | Performance at scale | Invisible until measured |
| Frontend | State synchronization | Complex interactions |
| Frontend | Bundle optimization | Trade-offs aren't obvious |
| Backend | Distributed caching | Invalidation is hard |
| Backend | Query optimization | Can't see without tools |
| Backend | API versioning | Breaking changes expensive |
| AI | Prompt debugging | Non-deterministic |
| AI | Agent reliability | Loops, hallucinations |
| AI | RAG evaluation | No standard metrics |

---

## What Success Looks Like

**3 months**: Shipped 2-3 major features, code reviewed by team, mentored 1-2 interns

**6 months**: Owns significant area, makes architectural decisions, leads discussions

**12 months**: Sets technical direction for team, mentors senior-ish engineers, drives excellence

---

## Questions to Ask

### For All
1. "What's the hardest technical problem you've solved?"
2. "How do you stay current with technology?"
3. "Tell me about a disagreement with your team."

### Frontend
1. "LCP is 4s. Walk me through debugging."
2. "Server vs Client Components - when to use which?"
3. "State shared across 20 components - how do you handle?"

### Backend
1. "Design API for 100k properties with 15 filters."
2. "Service is down - how do you handle?"
3. "SQL vs NoSQL - what for this platform?"

### AI
1. "How do you evaluate RAG quality?"
2. "LLM API is down - what happens?"
3. "How do you reduce hallucinations?"

---

*Propnet.ai - Building Real Estate's Future*
