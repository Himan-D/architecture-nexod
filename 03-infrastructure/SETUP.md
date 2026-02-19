# Development Setup

**One-time setup for new engineers. Should take < 2 hours.**

## Prerequisites

- macOS, Linux, or WSL2 on Windows
- Homebrew (macOS) or apt/yum (Linux)
- Git configured with your name/email

## 1. Install Tools

```bash
# macOS
brew install pyenv nvm docker postgresql@15 redis

# Ubuntu/Debian
sudo apt update
sudo apt install -y python3.11 python3.11-venv nodejs npm docker.io postgresql-15 redis-server

# Verify
python3 --version  # Should be 3.11+
node --version     # Should be 18+
docker --version   # Should be 24+
```

## 2. Clone Repo

```bash
git clone git@github.com:yourcompany/ai-platform.git
cd ai-platform
```

## 3. Backend Setup

```bash
cd backend

# Python environment
python3 -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -r requirements.txt

# Environment variables
cp .env.example .env
# Edit .env with your values

# Database
createdb ai_platform_dev
alembic upgrade head

# Run
uvicorn app.main:app --reload

# Test
curl http://localhost:8000/health
```

## 4. Frontend Setup

```bash
cd frontend

# Dependencies
npm install

# Environment
cp .env.example .env.local
# Edit .env.local

# Run
npm run dev

# Test
open http://localhost:3000
```

## 5. Docker (Optional but Recommended)

```bash
# Everything at once
docker-compose up

# Or individual services
docker-compose up postgres redis
docker-compose up backend
docker-compose up frontend
```

## 6. IDE Setup

**VS Code Extensions** (install these):
- Python (Microsoft)
- Pylance
- Black Formatter
- isort
- ESLint
- Prettier
- Tailwind CSS IntelliSense
- Thunder Client
- Docker
- GitLens

**Settings** (`settings.json`):
```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "[python]": {
    "editor.defaultFormatter": "ms-python.black-formatter",
    "editor.formatOnSave": true
  },
  "python.analysis.typeCheckingMode": "basic",
  "typescript.preferences.importModuleSpecifier": "relative"
}
```

## 7. Verify Setup

Run these commands, all should succeed:

```bash
# Backend tests
cd backend && pytest

# Frontend build
cd frontend && npm run build

# Type checking
cd frontend && npm run type-check

# Linting
cd backend && black --check .
cd backend && ruff check .
cd frontend && npm run lint
```

## 8. First Contribution

1. Create branch: `git checkout -b yourname/setup-test`
2. Make a small change (add your name to CONTRIBUTORS.md)
3. Commit: `git commit -m "chore: add [name] to contributors"`
4. Push: `git push origin yourname/setup-test`
5. Open PR
6. Get it merged

**You're ready to work.**

## Troubleshooting

**PostgreSQL won't start**:
```bash
# macOS
brew services start postgresql@15

# Linux
sudo service postgresql start
```

**Port already in use**:
```bash
# Find what's using port 8000
lsof -i :8000

# Kill it
kill -9 <PID>
```

**Docker permission denied**:
```bash
# Add user to docker group
sudo usermod -aG docker $USER
# Log out and back in
```

**Python version issues**:
```bash
# Use pyenv
pyenv install 3.11.7
pyenv local 3.11.7
```

## Common Commands

```bash
# Backend
cd backend && uvicorn app.main:app --reload    # Start server
cd backend && pytest                           # Run tests
cd backend && alembic revision --autogenerate -m "message"  # Create migration
cd backend && alembic upgrade head             # Run migrations
cd backend && black .                          # Format code
cd backend && ruff check .                     # Lint

# Frontend
cd frontend && npm run dev                     # Start dev server
cd frontend && npm run build                   # Production build
cd frontend && npm run test                    # Run tests
cd frontend && npm run lint                    # Lint
cd frontend && npm run type-check              # TypeScript check

# Docker
docker-compose up                              # Start all
docker-compose up -d                           # Start in background
docker-compose logs -f backend                 # View logs
docker-compose down                            # Stop all
docker-compose down -v                         # Stop and remove volumes
```

## Getting Help

- Stuck for > 30 mins? Ask in #engineering-help
- Environment issues? Ping @devops-team
- Check the wiki: https://wiki.company.com/engineering

---

**Total setup time**: ~1-2 hours for most people. If it takes longer, something's wrong. Ask for help.
