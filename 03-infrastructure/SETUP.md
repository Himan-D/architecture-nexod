# Dev Setup

**Should take < 2 hours.**

## Prerequisites

- macOS, Linux, or WSL2
- Git configured
- Terminal access

## 1. Install Tools

```bash
# macOS
brew install pyenv nvm docker postgresql@15 redis

# Ubuntu/Debian
sudo apt update
sudo apt install -y python3.11 python3.11-venv nodejs npm docker.io postgresql-15 redis-server
```

Verify:
```bash
python3 --version   # 3.11+
node --version      # 18+
docker --version    # 24+
```

## 2. Clone

```bash
git clone git@github.com:Himan-D/nexod-platform.git
cd nexod-platform
```

## 3. Backend

```bash
cd backend

# Python env
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Env
cp .env.example .env
# Edit .env

# DB
createdb nexod_dev
alembic upgrade head

# Run
uvicorn app.main:app --reload

# Test
curl http://localhost:8000/health
```

## 4. Frontend

```bash
cd frontend

npm install
cp .env.example .env.local

npm run dev
# Open http://localhost:3000
```

## 5. Docker (Alternative)

```bash
docker-compose up
```

## 6. VS Code Extensions

- Python, Pylance
- Black Formatter, isort
- ESLint, Prettier
- Tailwind CSS IntelliSense
- Docker, GitLens

Settings:
```json
{
  "editor.formatOnSave": true,
  "python.analysis.typeCheckingMode": "basic"
}
```

## 7. Verify

```bash
# Backend tests
cd backend && pytest

# Frontend build
cd frontend && npm run build

# Lint
cd backend && black --check .
cd backend && ruff check .
cd frontend && npm run lint
```

## 8. First PR

1. Branch: `git checkout -b yourname/test`
2. Change: Add yourself to CONTRIBUTORS.md
3. Commit: `git commit -m "chore: add [name]"`
4. Push: `git push origin yourname/test`
5. Open PR

Done.

## Troubleshooting

**Postgres won't start**:
```bash
brew services start postgresql@15  # macOS
sudo service postgresql start     # Linux
```

**Port in use**:
```bash
lsof -i :8000
kill -9 <PID>
```

**Python version**:
```bash
pyenv install 3.11.7
pyenv local 3.11.7
```

## Common Commands

```bash
# Backend
uvicorn app.main:app --reload   # Run
pytest                          # Test
alembic upgrade head            # Migrate
black .                        # Format

# Frontend
npm run dev                    # Run
npm run build                  # Build
npm run lint                   # Lint
```

## Help

Stuck > 30 min? Ask in #engineering on Google Chat.

---

himanshu.dixit@nexod.ai
