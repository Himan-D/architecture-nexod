# Senior Engineer Evaluation Framework

**Propnet.ai - AI-Powered Real Estate Platform**

---

## What We're Building

Propnet.ai is transforming real estate. Think Uber meets Zillow meets Claude.

Buyers, sellers, and agents all on one platform. AI handles:
- Property matching
- Scheduling showings
- Market analysis
- Follow-ups
- Negotiations

This is not a CRUD app. It's:
- Real-time (property changes instantly)
- Data-heavy (thousands of properties)
- AI-intensive (agents that do real work)
- Trust-critical (people's homes, significant money)

---

## What Makes Senior Different from Intern

**Intern**: Can ship defined tasks  
**Senior**: Defines what to build and how

**Intern**: Asks "how do I?"  
**Senior**: Asks "should we?"

**Intern**: Follows patterns  
**Senior**: Creates patterns

---

## Senior Frontend Engineer

### Experience Requirements

#### 1. Large Application Architecture
**What it means**: You've managed big codebases.

**The hard part**:
> "When do I split this component vs keep it together?"

Co-location isn't just files. It's about what loads together, what can be cached, what affects Time to Interactive.

**What we need**: You've shipped features in 10,000+ line codebases.

#### 2. State Management at Scale
**What it means**: Multiple data sources need to work together.

**The hard part**:
> "Why is my state out of sync?"

React Query for server state. Zustand for client. URL for shareable. Keeping them synchronized is hard. Knowing when to use which is harder.

**What we need**: You've designed state architecture for complex apps.

#### 3. Performance Optimization
**What it means**: Fast = good. Slow = users leave.

**The hard part**:
> "It's slow but I don't know why."

Performance profiling is its own skill. Chrome DevTools isn't enough. Need to understand what metrics matter.

**What we need**: You've taken something slow and made it fast.

### Propnet-Specific Challenges

**1. Maps Integration**
- Property markers (thousands)
- Clustering
- Different zoom levels

**Why hard**: Performance. Different detail at different zoom.

**2. Media Gallery**
- 20-50 photos per property
- Lazy loading
- Image optimization

**Why hard**: Can't load all at once. Storage costs.

**3. Real-Time Updates**
- Status changes (available → sold)
- Price changes
- Multiple viewers

**Why hard**: WebSocket management, optimistic updates.

**4. Complex Forms**
- Property listings (30+ fields)
- Multi-step wizards
- Draft saving

**Why hard**: State across steps. Validation complexity.

### Evaluation Rubric

| Category | Competent (3) | Strong (4) | Expert (5) |
|----------|---------------|------------|------------|
| Architecture | Can design features | Can design systems | Can design platform |
| Performance | Optimize known issues | Find unknown issues | Prevent issues |
| State Management | Uses patterns correctly | Chooses right pattern | Creates new patterns |
| Code Quality | Clean, tested | Documented, reviewed | Enables others |
| Leadership | Ships features | Mentors interns | Sets direction |

**Minimum**: 15+ / 25

---

## Senior Backend Engineer

### Experience Requirements

#### 1. Database Design at Scale
**What it means**: You know how data should be organized.

**The hard part**:
> "This query is slow but I don't know why."

EXPLAIN ANALYZE is your friend. ORM generates queries, not magic. Knowing when to denormalize.

**What we need**: You've designed schemas and optimized queries.

#### 2. API Design
**What it means**: Others consume your APIs. Make them good.

**The hard part**:
> "Should this be a new endpoint or query param?"

API design is about abstractions. Getting it wrong breaks clients.

**What we need**: You've designed APIs used by others.

#### 3. Distributed Systems
**What it means**: Things don't always work on one machine.

**The hard part**:
> "Why did the user see old data?"

Caching means data can be stale. Knowing when consistency matters vs when eventual is fine.

**What we need**: You've worked with caching, queues, distributed data.

#### 4. Security
**What it means**: Protect user data. Always.

**The hard part**:
> "Is this secure?"

Security isn't a feature. It's a mindset. Knowing attack vectors, common mistakes.

**What we need**: You've handled auth, authorization, API security.

### Propnet-Specific Challenges

**1. Search & Filtering**
- 15+ filter options
- Geo queries
- Full-text search

**Why hard**: Different filters need different indexes.

**2. Media Handling**
- Image upload/processing
- Thumbnail generation
- CDN integration

**Why hard**: Processing takes time. Storage costs.

**3. Scheduling**
- Showings
- Open houses
- Time zones

**Why hard**: Real-world conflicts. "Agent A says 2pm, Agent B says 2pm."

**4. Notifications**
- Email, SMS, push
- Scheduled notifications
- Opt-out handling

**Why hard**: Delivery guarantees. Rate limits.

### Evaluation Rubric

| Category | Competent (3) | Strong (4) | Expert (5) |
|----------|---------------|------------|------------|
| Database | Can write queries | Can optimize | Can design schema |
| API Design | RESTful APIs | Versioning, docs | Platform thinking |
| Distributed | Uses caching | Chooses approach | Creates patterns |
| Security | Follows best practices | Audits code | Threat modeling |
| Leadership | Ships features | Mentors interns | Sets direction |

**Minimum**: 15+ / 25

---

## Senior AI/ML Engineer

### Experience Requirements

#### 1. LLM Integration
**What it means**: Making AI work in production.

**The hard part**:
> "The model keeps giving bad answers."

Prompt debugging is hard because you can't step through. Need systematic approaches.

**What we need**: You've shipped LLM integrations.

#### 2. Agent Architecture
**What it means**: AI that takes actions, not just answers.

**The hard part**:
> "The agent keeps doing the wrong thing."

Agents can get into loops or misinterpret. Guardrails and monitoring critical.

**What we need**: You've built agent systems.

#### 3. Vector Databases
**What it means**: Making AI remember things.

**The hard part**:
> "RAG isn't returning good results."

Is it the embedding? Chunking? Retrieval? Prompt? Need systematic debugging.

**What we need**: You've implemented RAG systems.

### Propnet-Specific Challenges

**1. Property Matching**
- "I want something modern but not noisy"
- Learning from behavior
- "Why this property?"

**Why hard**: Preferences are complex. Attribution is hard.

**2. Conversation Agents**
- Remembering context
- Handling interruptions

**Why hard**: Context windows limited. Need smart summarization.

**3. Market Analysis**
- Price predictions
- Market trends

**Why hard**: Real estate is local. Data sparse.

**4. Content Generation**
- Property descriptions
- Marketing copy

**Why hard**: Must sound natural, not AI-generated.

### Evaluation Rubric

| Category | Competent (3) | Strong (4) | Expert (5) |
|----------|---------------|------------|------------|
| LLM Integration | API calls work | Prompt optimization | Research-level |
| Agent Systems | Simple agents | Complex orchestration | Production-grade |
| RAG/Vectors | Basic implementation | Evaluation + tuning | Novel approaches |
| ML Concepts | Basic statistics | Modeling | Research |
| Leadership | Ships features | Sets AI strategy | Company direction |

**Minimum**: 15+ / 25

---

## Technical Deep Dive

### What You Should Know

#### Profiling Tools

**Frontend**:
- Chrome DevTools Performance
- Lighthouse
- Bundle analyzer
- React DevTools Profiler

**Backend**:
- Py-spy
- pg_stat_statements
- Redis MONITOR

**AI**:
- LangSmith
- Custom logging

#### Must-Know Libraries

**Frontend**: Next.js 14, React Query, Zustand, Tailwind CSS, Playwright

**Backend**: FastAPI, SQLAlchemy 2.0, Alembic, Celery, Redis

**AI**: CrewAI, LangGraph, OpenAI SDK, Pinecone

---

## Common Questions

### For All

1. **"Walk me through how you'd design X"**
   - System thinking, trade-offs, scaling

2. **"What's the hardest technical problem you've solved?"**
   - Depth, problem-solving approach

3. **"How do you stay current with technology?"**
   - Learning habits, curiosity

4. **"Tell me about a time you disagreed with your team"**
   - Communication, collaboration

### For Frontend

1. **"How would you optimize a page with LCP of 4s?"**
2. **"When do you use Server vs Client Components?"**
3. **"How do you handle state shared across 10 components?"**

### For Backend

1. **"Design an API for 10,000 properties with 20 filters"**
2. **"How would you handle a service that's down?"**
3. **"SQL vs NoSQL for this use case?"**

### For AI

1. **"How would you evaluate if RAG is working?"**
2. **"What happens when LLM API is down?"**
3. **"How would you reduce hallucinations?"**

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

**After 3 months**:
- Shipped 2-3 major features
- Code reviewed by team
- Mentored 1-2 interns

**After 6 months**:
- Owns significant area
- Makes architectural decisions
- Leads technical discussions

**After 12 months**:
- Sets technical direction for team
- Mentors senior-ish engineers
- Drives technical excellence

---

## Apply

**Email**: himanshu.dixit@nexod.ai

Include:
1. GitHub
2. Resume or LinkedIn
3. Brief: What's the hardest technical problem you've solved? How?

Let's talk code.

---

*Propnet.ai - Building the Future of Real Estate*
