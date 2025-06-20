# CLAUDE.md - Eero Web UI Project Instructions

## Project Overview
Build a comprehensive web UI that surfaces rich data from an Eero network using the unofficial API endpoints. This project will fork and extend the community `eero-client` library with modern async capabilities and a full-stack web interface.

## Core Objectives

### 1. Library Enhancement
- Fork `343max/eero-client` (167⭐ Python library) to `github.com/JeremyWhittaker/eero-client-mod`
- Modernize with async/await using httpx instead of requests
- Add retry logic with tenacity for token expiry and error handling
- Package properly with pyproject.toml for pip installation
- Port advanced GraphQL queries from `schmittx/home-assistant-eero`

### 2. Web Service Architecture
- **Backend**: FastAPI with async support
  - `/devices` - Live device list (MAC, IP, RSSI, hostname, manufacturer)
  - `/stats` - Network statistics (uptime, bandwidth, firmware, activity data)
  - `/reboot/node/{id}` - Node reboot endpoint (POST, requires admin)
  - `/healthz` - Kubernetes readiness probe
  - WebSocket support for real-time updates

- **Frontend**: React + Vite SPA
  - Real-time device grid with WebSocket updates
  - Network topology visualization
  - Statistics dashboard
  - Tailwind CSS for styling

### 3. Infrastructure
- Docker Compose stack:
  - FastAPI service
  - Redis for caching/session storage
  - Celery worker for background tasks
  - Periodic device polling (60s intervals)
- GitHub Actions CI/CD:
  - Python matrix testing (3.10, 3.11)
  - Docker image builds
  - Automated testing with pytest
  - Type checking with mypy

## Technical Implementation

### Directory Structure
```
eero-client-mod/
├── eero_client_mod/        # Enhanced library
│   └── async_api.py        # Async wrapper
├── web/                    # Web application
│   ├── app/                # FastAPI backend
│   └── frontend/           # React frontend
├── compose.yaml            # Docker orchestration
├── Dockerfile
├── pyproject.toml          # Poetry dependencies
├── Makefile                # Dev commands
└── .github/
    └── workflows/          # CI/CD pipelines
```

### Development Workflow

1. **Initial Setup**
   ```bash
   gh repo fork 343max/eero-client --clone --remote
   git switch -c web-ui
   git push -u origin web-ui
   ```

2. **Key Commands**
   - `make dev` - Local development server
   - `make test` - Run test suite
   - `make compose-up` - Docker Compose stack

3. **API Endpoints**
   - GraphQL queries to `api-user.e2ro.com`
   - Token refresh automation
   - Connection pooling with httpx.AsyncClient

### Security Requirements
- Store credentials in Docker secrets or environment variables
- Never commit real `.env` files (use `.env.sample`)
- Implement detect-secrets pre-commit hook
- Use SSH key authentication for Git operations

## Development Guidelines

### Code Standards
- Follow Conventional Commits specification
- Python: ruff formatting, mypy type checking
- JavaScript: ESLint + Prettier
- 100% async/await in Python backend
- React functional components with hooks

### Testing Requirements
- Unit tests for all API endpoints
- Integration tests for GraphQL queries
- Frontend component testing
- Minimum 80% code coverage

### Documentation
- API documentation via FastAPI's automatic OpenAPI/Swagger
- README with setup instructions
- Inline code documentation for complex logic

## Deliverables

### Must Have
- [ ] Forked and enhanced eero-client library
- [ ] FastAPI backend with all specified endpoints
- [ ] React frontend with real-time updates
- [ ] Docker Compose development environment
- [ ] GitHub Actions CI/CD pipeline
- [ ] Draft PR on `web-ui` branch

### Nice to Have
- [ ] Prometheus metrics exporter (`/metrics`)
- [ ] OAuth login flow support
- [ ] Multi-network support (home + office)
- [ ] Mobile-responsive design

## Success Criteria
1. `make test` passes on Python 3.10 & 3.11
2. `docker compose up` serves UI at http://localhost:8000
3. Real-time device updates work via WebSocket
4. Node reboot functionality demonstrated
5. Clean, maintainable codebase following best practices

## References
- Base library: https://github.com/343max/eero-client
- GraphQL reference: https://github.com/schmittx/home-assistant-eero
- Eero API is unofficial (consumer tier has no official API)
- GraphQL endpoint: api-user.e2ro.com

---
*This instruction set is designed for Claude Code to efficiently implement the Eero Web UI project with clear objectives and technical requirements.*