# Development Standards

**Owner**: All engineers  
**Enforced by**: CI/CD, code review

## Code Quality Gates

All code must pass:
- [ ] Type checking (mypy, TypeScript strict)
- [ ] Linting (ruff, ESLint)
- [ ] Formatting (black, prettier)
- [ ] Tests (pytest, jest) - 80%+ coverage
- [ ] Security scan (Bandit, npm audit)
- [ ] No secrets in code (git-secrets)

## Backend Standards

### FastAPI Patterns

**Project Structure**:
```
app/
├── __init__.py
├── main.py              # App factory
├── config.py            # Settings
├── dependencies.py      # DI container
├── database.py          # DB connection
├── models/              # SQLAlchemy models
├── schemas/             # Pydantic schemas
├── routers/             # API routes
├── services/            # Business logic
├── tasks/               # Celery tasks
├── utils/               # Helpers
└── tests/
```

**Model Pattern**:
```python
# models/user.py
from sqlalchemy import Column, String, DateTime
from sqlalchemy.orm import relationship
from app.database import Base

class User(Base):
    __tablename__ = "users"
    
    id = Column(String, primary_key=True)
    email = Column(String, unique=True, index=True)
    created_at = Column(DateTime, default=datetime.utcnow)
    
    # Relationships
    tasks = relationship("Task", back_populates="owner")
```

**Router Pattern**:
```python
# routers/users.py
from fastapi import APIRouter, Depends, HTTPException
from sqlalchemy.ext.asyncio import AsyncSession

router = APIRouter(prefix="/users", tags=["users"])

@router.get("/{user_id}", response_model=UserResponse)
async def get_user(
    user_id: str,
    db: AsyncSession = Depends(get_db),
    current_user: User = Depends(get_current_user)
):
    user = await user_service.get_user(db, user_id)
    if not user:
        raise HTTPException(status_code=404, detail="User not found")
    return user
```

**Service Pattern**:
```python
# services/user_service.py
from sqlalchemy.ext.asyncio import AsyncSession
from sqlalchemy import select

async def get_user(db: AsyncSession, user_id: str) -> Optional[User]:
    result = await db.execute(select(User).where(User.id == user_id))
    return result.scalar_one_or_none()
```

### Type Hints (Required)

```python
# Good
def calculate_total(items: list[Item], tax_rate: float = 0.08) -> Decimal:
    subtotal: Decimal = sum(item.price for item in items)
    return subtotal * (1 + tax_rate)

# Bad
def calculate_total(items, tax_rate=0.08):
    return sum(item.price for item in items) * (1 + tax_rate)
```

### Error Handling

```python
# Custom exceptions
class NotFoundError(Exception):
    pass

class ValidationError(Exception):
    pass

# Exception handlers
@app.exception_handler(NotFoundError)
async def not_found_handler(request: Request, exc: NotFoundError):
    return JSONResponse(
        status_code=404,
        content={"detail": str(exc)}
    )
```

### Testing

```python
# tests/test_users.py
import pytest
from httpx import AsyncClient

@pytest.mark.asyncio
async def test_get_user(client: AsyncClient, db: AsyncSession):
    # Arrange
    user = await create_test_user(db, email="test@example.com")
    
    # Act
    response = await client.get(f"/users/{user.id}")
    
    # Assert
    assert response.status_code == 200
    assert response.json()["email"] == "test@example.com"
```

## Frontend Standards

### Component Structure

```
app/
├── layout.tsx           # Root layout
├── page.tsx             # Home page
├── (auth)/              # Route groups
│   ├── login/
│   └── signup/
├── api/                 # API routes
├── components/
│   ├── ui/              # Reusable UI (Button, Input)
│   ├── features/        # Feature components (TaskList)
│   └── layouts/         # Layout components (Sidebar)
├── hooks/               # Custom hooks
├── lib/                 # Utilities
├── stores/              # Zustand stores
└── types/               # TypeScript types
```

### Component Pattern

```typescript
// components/features/TaskList.tsx
'use client'

import { useQuery } from '@tanstack/react-query'
import { TaskCard } from './TaskCard'
import { fetchTasks } from '@/lib/api'

interface TaskListProps {
  userId: string
  status?: 'active' | 'completed'
}

export function TaskList({ userId, status = 'active' }: TaskListProps) {
  const { data: tasks, isLoading, error } = useQuery({
    queryKey: ['tasks', userId, status],
    queryFn: () => fetchTasks(userId, status),
  })
  
  if (isLoading) return <TaskListSkeleton />
  if (error) return <ErrorMessage error={error} />
  if (!tasks?.length) return <EmptyState />
  
  return (
    <div className="space-y-4">
      {tasks.map(task => (
        <TaskCard key={task.id} task={task} />
      ))}
    </div>
  )
}
```

### Reusable Components

**Button**:
```typescript
// components/ui/Button.tsx
import { ButtonHTMLAttributes, forwardRef } from 'react'
import { cn } from '@/lib/utils'

interface ButtonProps extends ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: 'primary' | 'secondary' | 'danger'
  size?: 'sm' | 'md' | 'lg'
  isLoading?: boolean
}

export const Button = forwardRef<HTMLButtonElement, ButtonProps>(
  ({ variant = 'primary', size = 'md', isLoading, className, children, ...props }, ref) => {
    return (
      <button
        ref={ref}
        className={cn(
          'rounded font-medium transition-colors',
          {
            'bg-blue-600 text-white hover:bg-blue-700': variant === 'primary',
            'bg-gray-200 text-gray-800 hover:bg-gray-300': variant === 'secondary',
            'bg-red-600 text-white hover:bg-red-700': variant === 'danger',
            'px-3 py-1.5 text-sm': size === 'sm',
            'px-4 py-2 text-base': size === 'md',
            'px-6 py-3 text-lg': size === 'lg',
          },
          'disabled:opacity-50 disabled:cursor-not-allowed',
          className
        )}
        disabled={isLoading || props.disabled}
        {...props}
      >
        {isLoading ? <Spinner /> : children}
      </button>
    )
  }
)
```

**Input**:
```typescript
// components/ui/Input.tsx
import { forwardRef, InputHTMLAttributes } from 'react'
import { cn } from '@/lib/utils'

interface InputProps extends InputHTMLAttributes<HTMLInputElement> {
  label?: string
  error?: string
}

export const Input = forwardRef<HTMLInputElement, InputProps>(
  ({ label, error, className, ...props }, ref) => {
    return (
      <div className="space-y-1">
        {label && (
          <label className="text-sm font-medium text-gray-700">
            {label}
          </label>
        )}
        <input
          ref={ref}
          className={cn(
            'w-full rounded border px-3 py-2',
            'focus:outline-none focus:ring-2 focus:ring-blue-500',
            error && 'border-red-500',
            className
          )}
          {...props}
        />
        {error && (
          <p className="text-sm text-red-600">{error}</p>
        )}
      </div>
    )
  }
)
```

### State Management

**Server State (React Query)**:
```typescript
// hooks/useTasks.ts
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query'

export function useTasks(userId: string) {
  return useQuery({
    queryKey: ['tasks', userId],
    queryFn: () => fetchTasks(userId),
    staleTime: 5 * 60 * 1000, // 5 minutes
  })
}

export function useCreateTask() {
  const queryClient = useQueryClient()
  
  return useMutation({
    mutationFn: createTask,
    onSuccess: (data) => {
      queryClient.invalidateQueries({ queryKey: ['tasks'] })
    },
  })
}
```

**Client State (Zustand)**:
```typescript
// stores/uiStore.ts
import { create } from 'zustand'

interface UIState {
  sidebarOpen: boolean
  toggleSidebar: () => void
  theme: 'light' | 'dark'
  setTheme: (theme: 'light' | 'dark') => void
}

export const useUIStore = create<UIState>((set) => ({
  sidebarOpen: true,
  toggleSidebar: () => set((state) => ({ sidebarOpen: !state.sidebarOpen })),
  theme: 'light',
  setTheme: (theme) => set({ theme }),
}))
```

### Form Handling

```typescript
// components/forms/TaskForm.tsx
'use client'

import { useForm } from 'react-hook-form'
import { zodResolver } from '@hookform/resolvers/zod'
import { z } from 'zod'

const taskSchema = z.object({
  title: z.string().min(1, 'Title is required'),
  description: z.string().optional(),
  dueDate: z.date().optional(),
})

type TaskFormData = z.infer<typeof taskSchema>

export function TaskForm({ onSubmit }: { onSubmit: (data: TaskFormData) => void }) {
  const {
    register,
    handleSubmit,
    formState: { errors, isSubmitting },
  } = useForm<TaskFormData>({
    resolver: zodResolver(taskSchema),
  })
  
  return (
    <form onSubmit={handleSubmit(onSubmit)} className="space-y-4">
      <Input
        label="Title"
        {...register('title')}
        error={errors.title?.message}
      />
      <Button type="submit" isLoading={isSubmitting}>
        Create Task
      </Button>
    </form>
  )
}
```

## API Integration

**API Client**:
```typescript
// lib/api/client.ts
import axios from 'axios'

export const api = axios.create({
  baseURL: process.env.NEXT_PUBLIC_API_URL,
  headers: {
    'Content-Type': 'application/json',
  },
})

// Request interceptor
api.interceptors.request.use((config) => {
  const token = localStorage.getItem('token')
  if (token) {
    config.headers.Authorization = `Bearer ${token}`
  }
  return config
})

// Response interceptor
api.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response?.status === 401) {
      window.location.href = '/login'
    }
    return Promise.reject(error)
  }
)
```

## Database Migrations (Alembic)

**Create Migration**:
```bash
cd backend
alembic revision --autogenerate -m "add user table"
```

**Migration Best Practices**:
- One logical change per migration
- Test migrations on copy of prod data
- Never delete columns with data
- Use transactions
- Backwards compatible when possible

**Example**:
```python
"""add user table

Revision ID: abc123
Revises: xyz789
Create Date: 2026-02-19

"""
from alembic import op
import sqlalchemy as sa

# revision identifiers
revision = 'abc123'
down_revision = 'xyz789'

def upgrade():
    op.create_table(
        'users',
        sa.Column('id', sa.String(), nullable=False),
        sa.Column('email', sa.String(), nullable=False),
        sa.Column('created_at', sa.DateTime(), nullable=False),
        sa.PrimaryKeyConstraint('id'),
        sa.UniqueConstraint('email')
    )
    op.create_index('ix_users_email', 'users', ['email'])

def downgrade():
    op.drop_index('ix_users_email')
    op.drop_table('users')
```

## Git Workflow

**Branch Naming**:
```
feature/user-authentication
bugfix/login-redirect
hotfix/security-patch
docs/api-examples
refactor/database-queries
```

**Commit Messages**:
```
feat: add user authentication
fix: redirect after login
docs: update API examples
refactor: optimize database queries
test: add user service tests
chore: update dependencies
```

**Pull Request Template**:
```markdown
## What
Brief description

## Why
Context and motivation

## How
Implementation details

## Testing
- [ ] Unit tests pass
- [ ] Integration tests pass
- [ ] Manual testing done

## Screenshots (if UI)

## Checklist
- [ ] Code follows style guide
- [ ] Self-review completed
- [ ] Comments added for complex logic
- [ ] Documentation updated
```

## Performance Standards

**Backend**:
- API response <200ms (p95)
- Database queries <50ms
- N+1 queries prohibited
- Use selectinload for relationships
- Cache expensive operations

**Frontend**:
- First Contentful Paint <1.8s
- Largest Contentful Paint <2.5s
- Time to Interactive <3.8s
- Bundle size <200KB (initial)
- Lazy load routes and heavy components

## Security Checklist

- [ ] No secrets in code (use env vars)
- [ ] Input validation (Pydantic, Zod)
- [ ] SQL injection prevention (parameterized queries)
- [ ] XSS prevention (React escapes by default)
- [ ] CSRF tokens for forms
- [ ] Rate limiting on APIs
- [ ] HTTPS only
- [ ] Secure cookies (HttpOnly, Secure, SameSite)
- [ ] Dependency scanning (npm audit, safety)

---

*Questions? Check examples in /examples folder or ask in #engineering*
