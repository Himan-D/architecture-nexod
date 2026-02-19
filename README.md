# Architecture Nexod - Platform Overview

**Repository**: https://github.com/Himan-D/architecture-nexod  
**Owner**: Himanshu Dixit (himanshu.dixit@nexod.ai)  
**Last Updated**: Feb 2026

## Quick Links

- [Core Architecture](./01-core/)
- [Team Structure & Hiring](./02-team/)
- [Infrastructure & DevOps](./03-infrastructure/)
- [Development Guidelines](./04-development/)
- [Policies & Standards](./05-policies/)

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Next.js 14, TypeScript, Tailwind CSS |
| Backend | FastAPI, Python 3.11 |
| Database | PostgreSQL (Prod), Supabase (Dev) |
| Auth | Clerk |
| Cache | Redis |
| Hosting | Vercel (Frontend), AWS ECS (Backend) |
| Monitoring | Sentry, Py-spy, Prometheus |

## Team Structure

**Layer 1 - Leadership**:
- Himanshu Dixit (Architect + Senior Backend)
- Senior Frontend Engineer (TBD)

**Layer 2 - Core Team**:
- 2 Backend Interns
- 2 Frontend Interns  
- 1 DevOps Intern

## Communication Stack

| Tool | Purpose | Why |
|------|---------|-----|
| **Gmail** | Official communication | Professional threads, external comms |
| **Google Docs** | Documentation, RFCs | Real-time collaboration, version history |
| **Trello** | Task management | Simple, visual, intern-friendly |
| **Slack** | Daily chat | Quick questions, notifications |
| **GitHub** | Code + Issues | Single source of truth |
| **Figma** | Design | Handoff to dev |
| **Loom** | Async video | Complex explanations without meetings |

## Folder Structure

```
├── 01-core/              # Architecture decisions
├── 02-team/              # Hiring, roles, expectations  
├── 03-infrastructure/    # DevOps, monitoring, setup
├── 04-development/       # Coding standards, components
└── 05-policies/          # AI usage, security, data
```

## Getting Started

1. Read [Team Roles](./02-team/ROLES.md)
2. Setup environment: [03-infrastructure/SETUP.md](./03-infrastructure/SETUP.md)
3. Join communication channels
4. Pick your first task from Trello

## Database Strategy

**Production**: AWS RDS PostgreSQL  
- Multi-AZ for HA
- Automated backups
- Read replicas for scale

**Development**: Supabase  
- Instant setup for interns
- Built-in auth (dev only)
- Real-time subscriptions
- Free tier sufficient

**Why 2 databases?**:
- Production needs AWS SLAs
- Dev needs speed and zero-config
- Prevents accidental prod data access
- Supabase real-time helps with local testing

## Key Principles

1. **Ship fast, iterate faster** - Weekly deploys minimum
2. **Monitor everything** - If it's not measured, it doesn't exist
3. **Reusable components** - Build once, use everywhere
4. **Clear ownership** - Every feature has an owner
5. **AI-assisted, human-approved** - All AI code reviewed

## Contact

- **Email**: himanshu.dixit@nexod.ai
- **Slack**: @himanshu
- **GitHub**: @Himan-D
