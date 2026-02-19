# Senior Engineer Evaluation Framework

**For**: Frontend Lead, Backend Lead, AI/ML Engineer  
**Platform**: Real Estate Agent Platform (Propnet-like)  
**Owner**: Himanshu Dixit

---

## What We're Building

Propnet.ai is an AI-powered real estate platform. It's Uber for real estate - connecting buyers, sellers, and agents with AI handling the matching, scheduling, and follow-ups.

This is not a simple CRUD app. It's:
- Real-time (property status changes instantly)
- Data-heavy (thousands of properties, images, messages)
- AI-intensive (agents that actually do work)
- Trust-critical (people's homes, lots of money)

---

## Senior vs Intern: What's Different

**Intern**: Can ship defined tasks  
**Senior**: Defines what to build and how

**Intern**: Asks "how do I?"  
**Senior**: Asks "should we?"

**Intern**: Follows patterns  
**Senior**: Creates patterns

---

## Senior Frontend Engineer

### Must Have Experience

**1. Large Application Architecture**
- Managed 10,000+ line codebase
- Code splitting, lazy loading
- Bundle optimization

**Hardest to master**:
> "When do I split this component vs keep it together?"
> 
> Understanding that co-location isn't just about files. It's about what loads together, what can be cached, what affects TTI (Time to Interactive).

**2. State Management at Scale**
- Multiple data sources (server, client, URL)
- Caching strategies
- Synchronization

**Hardest to master**:
> "Why is my state out of sync?"
> 
> When you have React Query for server state, Zustand for client state, URL for shareable state - keeping them in sync is hard. Knowing when to use which is harder.

**3. Performance Optimization**
- Core Web Vitals (LCP, FID, CLS)
- Bundle analysis
- Memory leak detection

**Hardest to master**:
> "It's slow but I don't know why."
> 
> Performance profiling is its own skill. Knowing Chrome DevTools isn't enough - you need to understand what metrics matter and what to optimize first.

### Should Have Built

**At least one of**:
- Real-time application (WebSocket, SSE)
- Large dashboard (100+ components)
- Design system (used by 5+ people)
- Performance optimization (LCP from 4s to 2s)

### Real Estate Specific Challenges

**1. Maps Integration**
- Google Maps/Mapbox
- Property markers (thousands on screen)
- Clustering, bounding
- Custom markers (sold, available, open house)

**Why hard**: Performance with many markers. Different zoom levels need different detail.

**2. Media Gallery**
- Image heavy (property photos)
- Lazy loading
- Image optimization
- Zoom, crop, rotate

**Why hard**: Hundreds of images per property. Can't load all at once.

**3. Real-time Property Updates**
- Status changes (available → under contract → sold)
- Price changes
- New listings
- Multiple viewers seeing same data

**Why hard**: WebSocket management, optimistic updates, conflict resolution.

**4. Complex Forms**
- Property listings (30+ fields)
- Multi-step wizards
- Draft saving
- Image uploads

**Why hard**: State management across steps. Validation complexity.

### Evaluation Rubric

| Category | Competent (3) | Strong (4) | Expert (5) |
|----------|---------------|------------|------------|
| **Architecture** | Can design features | Can design systems | Can design platform |
| **Performance** | Can optimize known issues | Can find unknown issues | Can prevent issues |
| **State Management** | Uses patterns correctly | Chooses right pattern | Creates new patterns |
| **Code Quality** | Clean, tested | Documented, reviewed | Enables others |
| **Leadership** | Ships features | Mentors interns | Sets technical direction |

**Minimum**: 15+ out of 25

---

## Senior Backend Engineer

### Must Have Experience

**1. Database Design at Scale**
- Complex relationships
- Query optimization
- Indexing strategies

**Hardest to master**:
> "This query is slow but I don't know why."
> 
> EXPLAIN ANALYZE is your friend. Understanding that ORM generates queries, not magic. Knowing when to denormalize.

**2. API Design**
- RESTful principles
- Versioning
- Documentation
- Rate limiting

**Hardest to master**:
> "Should this be a new endpoint or query param?"
> 
> API design is about abstractions. Getting it wrong means breaking clients later.

**3. Distributed Systems Concepts**
- Caching (multi-layer)
- Message queues
- eventual consistency

**Hardest to master**:
> "Why did the user see old data?"
> 
> Understanding that caching means data can be stale. Knowing when consistency matters vs when eventual is fine.

**4. Security**
- Authentication flows
- Authorization (RBAC)
- API security

**Hardest to master**:
> "Is this secure?"
> 
> Security isn't a feature, it's a mindset. Knowing attack vectors, common mistakes.

### Should Have Built

**At least one of**:
- High-traffic API (10k+ requests/day)
- Real-time system (WebSocket)
- Payment integration
- Third-party API integration

### Real Estate Specific Challenges

**1. Search & Filtering**
- 15+ filter parameters
- Geo queries (within X miles)
- Full-text search
- Sorting (price, date, relevance)

**Why hard**: Different filters need different indexing strategies. Geo queries are expensive.

**2. Media Handling**
- Image upload/processing
- Thumbnail generation
- CDN integration
- Storage costs

**Why hard**: Properties have 20-50 photos. Processing takes time. Storage costs add up.

**3. Scheduling**
- Open house scheduling
- Showing appointments
- Agent availability
- Time zones

**Why hard**: Real-world conflicts. Agent A says 2pm, Agent B says 2pm. Time zones.

**4. Notifications**
- Email, SMS, push
- Scheduled notifications
- Template management
- Opt-out handling

**Why hard**: Delivery guarantees. Rate limits. User preferences.

**5. Lead Management**
- Lead capture
- Assignment logic
- Follow-up automation
- Conversion tracking

**Why hard**: Sales workflows are complex. Attribution is hard.

### Evaluation Rubric

| Category | Competent (3) | Strong (4) | Expert (5) |
|----------|---------------|------------|------------|
| **Database** | Can write queries | Can optimize | Can design schema |
| **API Design** | RESTful APIs | Versioning, docs | Platform thinking |
| **Distributed** | Uses caching | Chooses right approach | Creates new patterns |
| **Security** | Follows best practices | Audits code | Threat modeling |
| **Leadership** | Ships features | Mentors interns | Sets technical direction |

**Minimum**: 15+ out of 25

---

## Senior AI/ML Engineer

### Must Have Experience

**1. LLM Integration**
- API integration (OpenAI, Anthropic)
- Prompt engineering
- Function calling
- Streaming

**Hardest to master**:
> "The model keeps giving bad answers."
> 
> Prompt debugging is hard because you can't step through it. Need systematic approaches to testing prompts.

**2. Agent Architecture**
- Tool definition
- Chain of thought
- Error handling
- Memory management

**Hardest to master**:
> "The agent keeps doing the wrong thing."
> 
> Agents can get into weird loops or misinterpret instructions. Guardrails and monitoring are critical.

**3. Vector Databases**
- Embedding generation
- Similarity search
- RAG implementation
- Chunking strategies

**Hardest to master**:
> "RAG isn't returning good results."
> 
> Is it the embedding? The chunking? The retrieval? The prompt? Need systematic debugging.

### Should Have Built

**At least one of**:
- Production LLM integration
- Agent system in production
- RAG system with evaluation

### Real Estate Specific Challenges

**1. Property Matching**
- User preferences → property matching
- Learning from user behavior
- Explainability ("why this property?")

**Why hard**: Preferences are complex ("near work but not noisy"). Attribution is hard.

**2. Conversation Agents**
- Remembering context
- Handling interruptions
- Multi-turn conversations

**Why hard**: LLM context windows are limited. Need to summarize or truncate history intelligently.

**3. Market Analysis**
- Price predictions
- Market trends
- Comparable analysis

**Why hard**: Real estate is local. Data can be sparse. Models need to handle cold start.

**4. Content Generation**
- Property descriptions
- Marketing copy
- Email templates

**Why hard**: Need to sound natural, not AI-generated. Brand voice consistency.

### Evaluation Rubric

| Category | Competent (3) | Strong (4) | Expert (5) |
|----------|---------------|------------|------------|
| **LLM Integration** | API calls work | Prompt optimization | Research-level |
| **Agent Systems** | Simple agents | Complex orchestration | Production-grade |
| **RAG/Vectors** | Basic implementation | Evaluation + tuning | Novel approaches |
| **ML Concepts** | Basic statistics | Modeling | Research |
| **Leadership** | Ships features | Sets AI strategy | Company AI direction |

**Minimum**: 15+ out of 25

---

## Common Senior Evaluation Questions

### For All

1. **"Walk me through how you'd design X"**
   - Looking for: System thinking, trade-offs, scaling considerations

2. **"What's the hardest technical problem you've solved?"**
   - Looking for: Depth of experience, problem-solving approach

3. **"How do you stay current with technology?"**
   - Looking for: Learning habits, curiosity

4. **"Tell me about a time you disagreed with your team"**
   - Looking for: Communication, collaboration, how they handle conflict

5. **"How would you mentor an intern?"**
   - Looking for: Leadership, patience, teaching ability

### For Frontend

1. **"How would you optimize a page with LCP of 4s?"**
   - Looking for: Understanding of Core Web Vitals, prioritization

2. **"When do you use Server Components vs Client Components?"**
   - Looking for: Next.js 14 understanding, trade-offs

3. **"How do you handle state that's shared across 10 components?"**
   - Looking for: State management patterns, when to lift vs context

### For Backend

1. **"Design an API that handles 10,000 properties with 20 filter options"**
   - Looking for: Query optimization, caching, pagination

2. **"How would you handle a service that's down?"**
   - Looking for: Resilience patterns, error handling

3. **"What's the difference between SQL and NoSQL for this use case?"**
   - Looking for: Trade-offs, when to use what

### For AI

1. **"How would you evaluate if your RAG system is working?"**
   - Looking for: Evaluation metrics, testing approach

2. **"What happens when the LLM API is down?"**
   - Looking for: Fallback strategies, error handling

3. **"How would you reduce hallucinations in property descriptions?"**
   - Looking for: Prompt engineering, validation, human-in-loop

---

## Hardest Topics for Seniors

| Role | Topic | Why It's Hard |
|------|-------|---------------|
| Frontend | Performance at scale | Invisible problems until you measure |
| Frontend | State synchronization | Complex interactions, hard to debug |
| Frontend | Bundle optimization | Trade-offs aren't obvious |
| Backend | Distributed caching | Invalidation is hard |
| Backend | Query optimization | Can't see what you don't measure |
| Backend | API versioning | Breaking changes are expensive |
| AI | Prompt debugging | Non-deterministic, hard to reproduce |
| AI | Agent reliability | Loops, hallucinations, timeouts |
| AI | RAG evaluation | No standard metrics |

---

## Technical Deep Dive: What You Should Know

### Profiling Tools

**Frontend**:
- Chrome DevTools Performance
- Lighthouse
- Bundle analyzer
- React DevTools Profiler

**Backend**:
- Py-spy (Python)
- pg_stat_statements (PostgreSQL)
- Redis MONITOR
- New Relic / Datadog

**AI**:
- LangSmith (LangChain)
- Weights & Biases
- Custom logging

### Must-Know Libraries

**Frontend**:
- Next.js 14
- React Query
- Zustand
- Tailwind CSS
- Playwright

**Backend**:
- FastAPI
- SQLAlchemy 2.0
- Alembic
- Celery
- Redis

**AI**:
- CrewAI
- LangGraph
- OpenAI SDK
- Pinecone

---

## What Success Looks Like

**After 3 months**:
- Shipped 2-3 major features
- Code reviewed by team
- Mentored 1-2 interns

**After 6 months**:
- Owns a significant area
- Makes architectural decisions
- Leads technical discussions

**After 12 months**:
- Sets technical direction for team
- Mentors senior-ish engineers
- Drives technical excellence

---

## Compensation Philosophy

We don't discuss specific numbers here. What matters:

- Market rate
- Your experience
- What you bring

We'll figure this out together.

---

## Apply

**Email**: himanshu.dixit@nexod.ai

Include:
1. GitHub
2. LinkedIn or resume
3. Brief: What's the hardest technical problem you've solved? How?

No cover letter. Let's talk code.

---

himanshu.dixit@nexod.ai
