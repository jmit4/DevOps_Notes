# Docker Compose

Managing multi-container applications with Docker Compose.

---

## What is Docker Compose?

Docker Compose lets you define and run multi-container Docker applications using a single YAML file (`docker-compose.yml`). It is ideal for local development environments.

---

## Basic Commands

```bash
docker compose up                   # Start all services (foreground)
docker compose up -d                # Start in detached mode
docker compose up --build           # Rebuild images before starting
docker compose down                 # Stop and remove containers, networks
docker compose down -v              # Also remove volumes
docker compose ps                   # List running services
docker compose logs                 # View logs from all services
docker compose logs -f web          # Follow logs from a specific service
docker compose exec web bash        # Open a shell in a running service
docker compose restart web          # Restart a specific service
docker compose pull                 # Pull latest images
docker compose build                # Build images
docker compose config               # Validate and view resolved compose file
```

---

## Example: Web App + Database

```yaml
# docker-compose.yml
services:
  web:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=development
      - DATABASE_URL=postgres://user:pass@db:5432/mydb
    volumes:
      - .:/app
      - /app/node_modules
    depends_on:
      db:
        condition: service_healthy

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: pass
      POSTGRES_DB: mydb
    volumes:
      - postgres_data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U user"]
      interval: 10s
      timeout: 5s
      retries: 5

volumes:
  postgres_data:
```

---

## Service Configuration Options

```yaml
services:
  myservice:
    image: nginx:latest          # Use an existing image
    build:                       # Or build from a Dockerfile
      context: ./app
      dockerfile: Dockerfile.prod
    ports:
      - "8080:80"                # host:container
    environment:
      - MY_VAR=value
    env_file:
      - .env                     # Load from a .env file
    volumes:
      - ./data:/data             # Bind mount
      - my-volume:/var/lib/data  # Named volume
    networks:
      - frontend
      - backend
    restart: unless-stopped      # always | on-failure | no
    command: ["node", "server.js"]
    depends_on:
      - db
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost/health"]
      interval: 30s
      timeout: 10s
      retries: 3

volumes:
  my-volume:

networks:
  frontend:
  backend:
```

---

## Environment Variables

Use a `.env` file (never commit secrets):

```env
POSTGRES_PASSWORD=supersecret
APP_PORT=3000
```

Reference in `docker-compose.yml`:

```yaml
environment:
  - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
ports:
  - "${APP_PORT}:3000"
```

---

## Profiles (Compose v2)

Use profiles to selectively start services:

```yaml
services:
  web:
    image: nginx
  debug:
    image: busybox
    profiles:
      - debug
```

```bash
docker compose --profile debug up
```

---

## Override Files

Layer configuration with override files:

```bash
# docker-compose.yml          ← base config
# docker-compose.override.yml ← automatically applied locally
# docker-compose.prod.yml     ← production overrides

docker compose -f docker-compose.yml -f docker-compose.prod.yml up -d
```
