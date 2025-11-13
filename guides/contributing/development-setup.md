# Development Setup Guide

Welcome to the DeployForge development team! This guide walks you through setting up your local environment for contributing to our multi-cloud deployment orchestrator. 

## Prerequisites

1. **Git**  
   Required for version control.  
   Install: [https://git-scm.com/downloads](https://git-scm.com/downloads)

2. **Docker & Docker Compose**  
   Used for containerized services.  
   Minimum versions: 
   - Docker Engine 20.10+
   - Docker Compose 2.17+  
   Install: [https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/)

3. **Go 1.21+**  
   Required for backend development.  
   Install: [https://go.dev/dl/](https://go.dev/dl/)

4. **Node.js 18+ & npm 9+**  
   Required for web interface development.  
   Install: [https://nodejs.org/](https://nodejs.org/)

## Environment Setup

### 1. Repository Setup
```bash
git clone https://github.com/deployforge/deployforge.git
cd deployforge
```

### 2. Configure Environment Variables
Create `.env` file from template:
```bash
cp .env.example .env
```
Edit `.env` to match your local configuration, particularly these values:
```ini
APP_ENV=development
POSTGRES_URL=postgres://deployforge:devpassword@localhost:5432/deployforge_dev?sslmode=disable
```

### 3. Database Setup
Start PostgreSQL using Docker:
```bash
docker compose up -d postgres
```

Initialize database schema:
```bash
go run cmd/migrate/main.go up
```

## Dependency Installation

### Backend Dependencies
```bash
go mod download
```

### Frontend Dependencies
```bash
cd web
npm install
cd ..
```

## Running the Application

### Start Backend Services
```bash
go run cmd/server/main.go
```

### Start Frontend Development Server
```bash
cd web
npm run dev
```

## Development Workflow

### Testing
Run unit tests:
```bash
go test ./... -short
```

Run integration tests (requires running database):
```bash
go test ./... -tags=integration
```

### Code Quality Tools
Format Go code:
```bash
gofmt -w .
```

Lint TypeScript code:
```bash
cd web
npm run lint
```

### Debugging
Attach Delve debugger:
```bash
dlv debug cmd/server/main.go
```

## Documentation Generation
Build API documentation:
```bash
swag init -g cmd/server/main.go
```

## Collaboration Guidelines

1. **Branch Naming**  
   Use format: `feature/your-feature` or `bugfix/issue-description`

2. **Commit Messages**  
   Follow [Conventional Commits](https://www.conventionalcommits.org/) specification

3. **Pull Requests**  
   - Reference related GitHub issues
   - Include test coverage report
   - Update documentation when applicable

## Troubleshooting

**Common Issues:**
- Port Conflicts: Check for processes using ports 5432 (PostgreSQL), 8080 (API), 3000 (Web)
SVG
- Database Connection Errors: Verify Postgres container is running and `.env` matches container credentials
- Dependency Conflicts: Delete `go.sum`/`node_modules` and reinstall dependencies

**Need Help?**  
Join our #developers channel on [community.deployforge.io](https://community.deployforge.io)