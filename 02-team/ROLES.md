# Team Structure & Roles

**Owner**: Himanshu Dixit  
**Review Date**: Monthly

## Team Layers

### Layer 1: Leadership (2 people)

**Himanshu Dixit - Architect & Senior Backend Lead**
- System architecture decisions
- Code review for all backend PRs
- Database design and optimization
- Production incidents (P0/P1)
- Mentoring backend interns
- AI integration strategy

**Senior Frontend Lead (TBD)**
- Frontend architecture decisions
- Component library ownership
- Code review for all frontend PRs
- Performance optimization
- Mentoring frontend interns
- UX/UI final decisions

**Expectations**:
- 5+ years experience
- Can ship production code independently
- Makes architectural decisions
- Available for escalations
- Writes technical RFCs

### Layer 2: Interns (5 people)

**Backend Interns (2)**
- Report to: Himanshu Dixit
- Build API endpoints
- Database migrations
- Background job implementation
- Testing (80%+ coverage)

**Frontend Interns (2)**
- Report to: Senior Frontend Lead
- Build UI components
- Implement features
- Write E2E tests
- Responsive design

**DevOps Intern (1)**
- Report to: Himanshu Dixit
- CI/CD pipeline maintenance
- Infrastructure as code
- Monitoring setup
- Cost optimization

**Expectations**:
- Ship code weekly
- Ask questions after 30 min of trying
- Participate in code reviews
- Document what you learn
- Convert to full-time if strong

## Communication Protocol

### Gmail (Official)
- External stakeholders
- Important decisions
- Weekly summaries
- Contract/legal stuff

**Template**:
```
Subject: [PROJECT] Brief description

Context:
Decision needed:
Options:
Recommendation:
Timeline:
```

### Google Docs (Documentation)
- RFCs (Request for Comments)
- Architecture decisions
- Post-mortems
- Playbooks

**Structure**:
- Use headers (H1, H2, H3)
- Table of contents
- Comment for questions
- Version history tracks changes

### Trello (Task Management)

**Boards**:
1. **Backlog** - Ideas, future work
2. **This Week** - Current sprint
3. **In Progress** - Being worked on
4. **Review** - PRs open
5. **Done** - Shipped

**Card Format**:
```
Title: [Backend] Add user authentication
Description:
- What: Implement JWT auth
- Why: Security requirement
- Acceptance Criteria:
  - [ ] Login endpoint works
  - [ ] Tests pass
  - [ ] Documentation updated
- Estimation: 3 days
- Owner: @intern-name
```

**Rules**:
- Every task has an owner
- Move cards when status changes
- Add labels (backend/frontend/devops/urgent)
- Attach PR links
- Archive done cards weekly

### Slack (Daily Chat)

**Channels**:
- `#general` - Announcements
- `#backend` - Backend discussions
- `#frontend` - Frontend discussions  
- `#devops` - Infrastructure
- `#help` - Questions (mention @here if urgent)
- `#random` - Non-work

**Etiquette**:
- Use threads for discussions
- @mention sparingly
- Share screenshots, not "it's broken"
- Loom videos for complex issues
- React with 👀 when reviewing

### GitHub (Code & Issues)

**Issues**:
- Bug reports
- Feature requests
- Technical debt

**Pull Requests**:
- Require 1 approval minimum
- Link to Trello card
- Include screenshots for UI
- All tests must pass
- No merge conflicts

## Meeting Schedule

| Meeting | When | Who | Duration |
|---------|------|-----|----------|
| Standup | Daily 10am | Everyone | 15 min |
| Sprint Planning | Monday 11am | Everyone | 30 min |
| Code Review | Wednesday 2pm | Everyone | 45 min |
| 1:1s | Friday (async) | Interns + Leads | 30 min each |
| Retro | Last Friday | Everyone | 30 min |

**Standup Format**:
1. What I shipped yesterday
2. What I'm working on today
3. Blockers (if any)

## Escalation Path

**Level 1**: Ask in Slack #help
**Level 2**: Mention @himanshu or @senior-frontend
**Level 3**: 1:1 meeting
**Level 4**: Emergency → Call Himanshu

## Documentation Ownership

| Document | Owner | Review Frequency |
|----------|-------|------------------|
| Architecture decisions | Himanshu | As needed |
| API documentation | Backend intern on rotation | Weekly |
| Component library | Senior Frontend | Bi-weekly |
| Runbooks | DevOps intern | Monthly |
| This roles doc | Himanshu | Monthly |

## Performance Review

**Interns** (Week 4, 8, 12):
- Code quality
- Independence
- Communication
- Learning speed

**Conversion Criteria**:
- Ships production code by week 8
- Handles feedback gracefully
- Mentors newer interns
- Proposes improvements

## Tools Access

**Day 1 Setup**:
- Gmail account (himanshu.dixit@nexod.ai invites)
- Google Drive folder
- Trello board invite
- Slack workspace
- GitHub organization
- Figma (view access)
- Sentry (read-only initially)
- Vercel (read-only initially)
- Supabase (dev database)

**Week 2** (if performing well):
- Production read access
- Sentry write access
- Deploy permissions (staging)
- Admin on Trello

## Onboarding Checklist

**Before Start**:
- [ ] Laptop ordered (if needed)
- [ ] Accounts created
- [ ] Trello board access
- [ ] First week tasks prepared

**Day 1**:
- [ ] Environment setup complete
- [ ] First PR merged
- [ ] Introduced to team
- [ ] Added to all channels

**Week 1**:
- [ ] First feature shipped
- [ ] Documentation read
- [ ] 1:1 with mentor
- [ ] Feedback on onboarding

---

*Questions? Slack @himanshu or email himanshu.dixit@nexod.ai*
