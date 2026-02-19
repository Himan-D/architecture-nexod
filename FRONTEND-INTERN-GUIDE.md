# Frontend Intern Hiring Guide

**Role**: Frontend Engineering Intern (2 positions)  
**Duration**: 12 weeks  
**Start Date**: Flexible  
**Location**: Remote (overlap 10am-4pm EST)

## What You'll Build

Production UI components. Real user-facing features. You'll:
- Build React components that thousands of users will see
- Implement complex UI patterns (drag-drop, real-time updates, data grids)
- Optimize for performance (bundle size, render times)
- Write E2E tests that catch bugs before users do
- Deploy to production with feature flags

## Minimum Requirements

Don't apply if you can't check these:

**JavaScript/TypeScript**:
- Built something with React (hooks, not just class components)
- Understand `useState`, `useEffect`, custom hooks
- Can explain props vs state
- Used TypeScript (types, interfaces, basic generics)
- Know ES6+ syntax (arrow functions, destructuring, spread)

**React**:
- Built components from scratch
- Understand component lifecycle
- Handled forms (controlled inputs, validation)
- Made API calls (fetch/axios)
- Handled loading and error states

**CSS**:
- Built responsive layouts (media queries)
- Used Flexbox and CSS Grid
- Know CSS specificity
- Used a CSS framework (Tailwind, Material-UI, etc.)

**Next.js** (or willing to learn fast):
- Understand file-based routing
- Know difference between Client and Server Components
- Used `getServerSideProps` or App Router data fetching

**Git**:
- Used branches
- Made pull requests
- Resolved conflicts
- Understand why commit messages matter

**Browser DevTools**:
- Console for debugging
- Network tab to inspect requests
- Elements tab to debug styles
- Performance profiling (bonus)

## Nice to Have

- Built with Next.js 14 App Router
- Used Zustand, Redux, or React Query
- Implemented authentication flows
- Written tests (Jest, React Testing Library)
- Used Framer Motion or similar
- Built a design system
- PWA experience

## Stack You'll Use

```
Next.js 14 (App Router)
React 18
TypeScript 5
Tailwind CSS 3
Zustand (state)
TanStack Query (server state)
React Hook Form + Zod
shadcn/ui components
Framer Motion
@tanstack/react-table
Recharts (charts)
Playwright (E2E testing)
Sentry (error tracking)
```

## What We Expect

**Week 1-2**: Get oriented
- Local environment running
- First PR (UI component or bug fix)
- Understand our component architecture
- Learn our design system

**Week 3-6**: Build features
- Implement full pages/features
- Handle responsive design
- Add error states and loading skeletons
- Write component tests

**Week 7-10**: Complex work
- Real-time features (WebSockets)
- Complex forms with validation
- Performance optimization
- Build reusable component library

**Week 11-12**: Polish & ship
- Complete major feature
- E2E test coverage
- Accessibility audit
- Present to team

## Interview Process

**Round 1: Coding Challenge (90 mins)**

Build a UI with:
- Form with validation (email, password)
- API integration (login endpoint)
- Loading and error states
- Responsive design (mobile + desktop)

We care about:
- Component structure
- Type safety
- Error handling
- UI polish (not pixel-perfect, but thoughtful)
- README with setup

**Round 2: Technical Interview (45 mins)**
- Review your challenge
- React patterns question
- System design (design a complex dashboard)
- CSS/layout question

**Round 3: Culture Fit (30 mins)**
- Show us a UI you built
- How do you handle design feedback?
- Questions for us

## Sample Interview Questions

1. **What's the difference between Server Component and Client Component in Next.js 14? When would you use each?**

2. **You're building a form with 20 fields. How do you manage state and validation?**
   (Looking for: React Hook Form, Zod/Yup, controlled vs uncontrolled)

3. **Your app is slow to render. How do you debug?** 
   (Looking for: React DevTools Profiler, bundle analysis, useMemo/useCallback when appropriate)

4. **Explain `useEffect` cleanup function. When do you need it?**

5. **How do you handle API errors in the UI?**
   (Looking for: Error boundaries, toast notifications, retry logic)

6. **You need to share state between two distant components. What are your options?**
   (Looking for: Context, Zustand/Redux, URL state, lifting state up)

7. **What's wrong with this code?**
   ```typescript
   const UserList = () => {
     const [users, setUsers] = useState([])
     
     fetch('/api/users').then(res => res.json()).then(data => {
       setUsers(data)
     })
     
     return <div>{users.map(u => <p>{u.name}</p>)}</div>
   }
   ```
   (Answer: Fetch on every render, infinite loop, missing useEffect, no cleanup, no error handling, no loading state)

## Red Flags

- Can't explain what React re-rendering means
- Never heard of TypeScript
- Can't center a div (seriously, basic CSS)
- Doesn't test on mobile
- Copy-pastes from StackOverflow without understanding
- Gets defensive about design feedback

## Green Flags

- Has a portfolio with live demos
- Contributed to open source UI libraries
- Built side projects with Next.js 14
- Writes about frontend performance
- Cares about accessibility (ARIA, keyboard nav)
- Pays attention to details (spacing, alignment)

## Compensation

- **Stipend**: $X,XXX/month
- **Equipment**: Laptop if needed
- **Learning**: Courses, conferences, design tools
- **Conversion**: Full-time offers for top performers

## Apply

Send to: engineering-jobs@company.com

Include:
1. Resume
2. GitHub/GitLab link
3. Link to live project (deployed, not just local)
4. 2-3 sentences on why this role

**No cover letters.**

---

## What Success Looks Like

**Week 6**: Ships features independently, writes tests, accepts feedback gracefully

**Week 12**: Ships production features, improves our component library, proposes UX improvements

---

*Last updated: Feb 2026*
