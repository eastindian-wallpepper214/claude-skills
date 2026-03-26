---
name: docker-setup
description: Dockerfile and docker-compose setup with multi-stage builds, health checks, and dev/prod configs
category: development
tags: [docker, containers, docker-compose, devops, deployment]
author: claude-skills
version: 1.0.0
---

# Docker Setup

Create a production-ready Docker configuration with multi-stage builds, docker-compose for development and production, health checks, and security best practices.

## Execution Steps

### 1. Analyze the Application

Determine:
- Language and runtime (Node.js, Python, Go, etc.)
- Package manager (npm, yarn, pnpm, pip, poetry)
- Build step required? (TypeScript compilation, webpack, etc.)
- Entry point command
- Required environment variables
- Ports exposed
- External service dependencies (database, cache, message queue)
- Static assets to serve

### 2. Create Multi-Stage Dockerfile

Use multi-stage builds to minimize the final image size:

```dockerfile
# Stage 1: Dependencies
FROM node:20-alpine AS deps
WORKDIR /app
COPY package.json package-lock.json ./
RUN npm ci --only=production && \
    cp -R node_modules /prod_modules && \
    npm ci

# Stage 2: Build
FROM node:20-alpine AS build
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN npm run build

# Stage 3: Production
FROM node:20-alpine AS production
WORKDIR /app

RUN addgroup -g 1001 appgroup && \
    adduser -u 1001 -G appgroup -s /bin/sh -D appuser

COPY --from=deps /prod_modules ./node_modules
COPY --from=build /app/dist ./dist
COPY package.json ./

ENV NODE_ENV=production
USER appuser
EXPOSE 3000

HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
  CMD wget --no-verbose --tries=1 --spider http://localhost:3000/health || exit 1

CMD ["node", "dist/index.js"]
```

Key principles:
- Use Alpine base images for smaller size
- Copy dependency files first (layer caching for fast rebuilds)
- Run as non-root user
- Include HEALTHCHECK instruction
- Set NODE_ENV in the image, not just at runtime
- Do not copy source code into the production stage

### 3. Create .dockerignore

```
node_modules
dist
build
.git
.github
.env
.env.*
*.md
*.log
coverage
.nyc_output
tests
__tests__
.vscode
.idea
Dockerfile
docker-compose*.yml
.dockerignore
```

### 4. Create docker-compose.yml for Development

```yaml
services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
      target: deps  # Use deps stage for dev (no build step)
    volumes:
      - .:/app
      - /app/node_modules  # Prevent host node_modules from overriding
    ports:
      - "${APP_PORT:-3000}:3000"
    environment:
      - NODE_ENV=development
      - DATABASE_URL=postgresql://postgres:postgres@db:5432/appdb
      - REDIS_URL=redis://redis:6379
    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_healthy
    command: npm run dev

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: appdb
    ports:
      - "${DB_PORT:-5432}:5432"
    volumes:
      - pgdata:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 5s
      timeout: 3s
      retries: 5

  redis:
    image: redis:7-alpine
    ports:
      - "${REDIS_PORT:-6379}:6379"
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 5s
      timeout: 3s
      retries: 5

volumes:
  pgdata:
```

### 5. Create docker-compose.prod.yml Override

```yaml
services:
  app:
    build:
      target: production
    volumes: []  # No source mounting in production
    command: []  # Use CMD from Dockerfile
    restart: unless-stopped
    deploy:
      resources:
        limits:
          memory: 512M
          cpus: "1.0"
    logging:
      driver: json-file
      options:
        max-size: "10m"
        max-file: "3"

  db:
    environment:
      POSTGRES_PASSWORD: ${DB_PASSWORD}  # From secure env
    volumes:
      - pgdata:/var/lib/postgresql/data
    restart: unless-stopped

  redis:
    restart: unless-stopped
    command: redis-server --requirepass ${REDIS_PASSWORD}
```

Usage: `docker compose -f docker-compose.yml -f docker-compose.prod.yml up -d`

### 6. Create Environment Template

Create `.env.example` with all required variables:

```
APP_PORT=3000
NODE_ENV=development
DATABASE_URL=postgresql://postgres:postgres@db:5432/appdb
REDIS_URL=redis://redis:6379
# Production only (set in deployment secrets):
# DB_PASSWORD=
# REDIS_PASSWORD=
# JWT_SECRET=
```

### 7. Security Checklist

- [ ] Running as non-root user (USER instruction)
- [ ] No secrets baked into the image (use env vars or mounted secrets)
- [ ] Base image pinned to specific version (not `latest`)
- [ ] .dockerignore excludes .env, .git, tests, docs
- [ ] HEALTHCHECK defined for orchestrator integration
- [ ] Production image contains only runtime dependencies (no devDependencies)
- [ ] Resource limits set in production compose
- [ ] Logging configured with rotation
- [ ] No unnecessary packages installed (no build tools in final stage)

### 8. Useful Commands

Document these for the team:

```bash
# Development
docker compose up -d              # Start all services
docker compose logs -f app        # Follow app logs
docker compose exec app sh        # Shell into running container
docker compose down -v            # Stop and remove volumes

# Production
docker compose -f docker-compose.yml -f docker-compose.prod.yml build
docker compose -f docker-compose.yml -f docker-compose.prod.yml up -d

# Debugging
docker compose ps                 # Check service status
docker compose exec db psql -U postgres -d appdb  # Database shell
docker stats                      # Resource usage
```
