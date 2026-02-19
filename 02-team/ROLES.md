# Team Structure

**Owner**: Himanshu Dixit  
**Review**: Monthly

## Layers

### Layer 1: Leadership (2)

**Himanshu Dixit - Architect & Agent Lead**
- System design, agent architecture (CrewAI, LangGraph)
- Database design, optimization
- Backend code review
- Production incidents (P0/P1)
- Mentors backend interns
- AI strategy, prompt engineering

**Senior Frontend Lead**
- Frontend architecture, design system
- Figma MCP workflows, rapid prototyping
- Component library ownership
- Frontend code review
- Mentors frontend interns
- UX/UI decisions

**Requirements**: 5+ years, ships independently, writes RFCs

### Layer 2: Interns (5)

**Backend Interns (2)**
- Report to: Himanshu
- Build APIs, database migrations
- Agent integration (CrewAI/LangGraph)
- Background jobs (Celery)
- Tests (80%+ coverage)

**Frontend Interns (2)**
- Report to: Senior Frontend Lead
- Build UI components
- Figma MCP integration
- Rapid prototyping
- E2E tests

**DevOps Intern (1)**
- Report to: Himanshu
- CI/CD, infrastructure
- Monitoring setup
- Performance optimization

**Requirements**: Ship weekly, ask after 30 min stuck, document learning

## Communication

### Gmail
- External stakeholders
- Important decisions
- Weekly summaries
- Legal/contracts

### Google Chat
**Spaces**:
- `Engineering - General` - Announcements
- `Backend` - API, database, agents
- `Frontend` - UI, components
- `DevOps` - Infrastructure
- `Help` - Questions (tag @himanshu if urgent)

**Etiquette**:
- Use threads
- @mention sparingly
- Share screenshots
- Loom for complex issues

### Google Drive
**Structure**:
```
Nexod Engineering/
├── 01-Architecture/       # RFCs, ADRs
├── 02-Runbooks/          # Incident response
├── 03-Agents/            # Agent designs
├── 04-Onboarding/        # New hire docs
└── 05-Meeting Notes/     # Weekly notes
```

**Naming**: `YYYY-MM-DD - Document Title`  
**Comments**: Use for questions  
**Version**: History tracks changes

### Trello

**Boards**:
- `Backlog` - Future work
- `This Week` - Current sprint
- `In Progress` - Active
- `Review` - PRs open
- `Done` - Shipped

**Card format**:
```
[Backend] Add CrewAI research agent

What: Build agent that researches topics
Why: Automate research workflows
Acceptance:
- [ ] Agent executes research
- [ ] Results stored in DB
- [ ] Tests pass
- [ ] Docs updated
Est: 5 days
Owner: @intern
```

### GitHub
- Issues for bugs/features
- PRs require 1 approval
- Link Trello cards
- Include screenshots

## Meeting Schedule

| Meeting | When | Duration |
|---------|------|----------|
| Standup | Daily 10am | 15 min |
| Sprint Planning | Monday 11am | 30 min |
| Code Review | Wednesday 2pm | 45 min |
| 1:1s | Friday async | 30 min |
| Retro | Last Friday | 30 min |

## Escalation

1. Google Chat `#help`
2. Tag @himanshu or @senior-frontend
3. 1:1 meeting
4. Emergency: Call Himanshu

## Performance Review

**Interns** (Week 4, 8, 12):
- Code quality, independence, communication
- **Conversion**: Ship prod code by week 8, mentor others

## Onboarding Checklist

**Day 1**:
- [ ] Gmail, Google Drive, Chat access
- [ ] Trello, GitHub, Figma access
- [ ] Environment setup
- [ ] First PR merged

**Week 1**:
- [ ] First feature shipped
- [ ] Read documentation
- [ ] 1:1 with mentor

---

Questions: himanshu.dixit@nexod.ai
