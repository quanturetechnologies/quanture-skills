---
name: docker
description: Builds, configures, and manages Docker containers and Docker Compose services following Quanture standards. Use when creating Dockerfiles, docker-compose.yml files, setting up containerized development environments, or deploying services with Docker.
---

# Docker — Quanture Standards

## Dockerfile (Python)

```dockerfile
# Use specific version — never use :latest
FROM python:3.11-slim

# Security: run as non-root user
RUN useradd --create-home appuser
WORKDIR /app

# Install deps first (layer caching)
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

# Switch to non-root
USER appuser

EXPOSE 8000
CMD ["uvicorn", "src.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

## Dockerfile (Java/Spring Boot)

```dockerfile
FROM eclipse-temurin:17-jre-alpine

RUN addgroup -S appgroup && adduser -S appuser -G appgroup
WORKDIR /app

COPY target/*.jar app.jar

USER appuser
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

## docker-compose.yml pattern

```yaml
version: "3.9"

services:
  app:
    build: .
    ports:
      - "8000:8000"
    environment:
      - DB_HOST=db
      - DB_NAME=appdb
      - DB_USER=appuser
      - DB_PASSWORD=${DB_PASSWORD}   # from .env file
    depends_on:
      db:
        condition: service_healthy
    restart: unless-stopped

  db:
    image: mysql:8.0
    environment:
      MYSQL_DATABASE: appdb
      MYSQL_USER: appuser
      MYSQL_PASSWORD: ${DB_PASSWORD}
      MYSQL_ROOT_PASSWORD: ${DB_ROOT_PASSWORD}
    volumes:
      - db_data:/var/lib/mysql
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
      interval: 10s
      timeout: 5s
      retries: 5

volumes:
  db_data:
```

## .env pattern

```bash
# .env.example — commit this
DB_PASSWORD=
DB_ROOT_PASSWORD=

# .env — NEVER commit, add to .gitignore
DB_PASSWORD=actualpassword
DB_ROOT_PASSWORD=rootpassword
```

## Security rules

- **Never** use `latest` tag — pin specific versions
- **Never** run as root — create and use a non-root user
- **Never** put secrets in Dockerfile or docker-compose.yml — use `.env`
- Use `--no-cache-dir` with pip to reduce image size
- Use `slim` or `alpine` base images when possible
- Scan images: `docker scout cves <image>`

## Common commands

```bash
# Build
docker build -t myapp:1.0.0 .

# Run with env file
docker run --env-file .env -p 8000:8000 myapp:1.0.0

# Compose
docker compose up -d          # start in background
docker compose logs -f app    # follow logs
docker compose down -v        # stop + remove volumes

# Debug running container
docker exec -it <container> sh
```

## Multi-stage build (smaller images)

```dockerfile
# Build stage
FROM maven:3.9-eclipse-temurin-17 AS builder
WORKDIR /build
COPY . .
RUN mvn package -DskipTests

# Runtime stage — smaller final image
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app
COPY --from=builder /build/target/*.jar app.jar
RUN adduser -S appuser
USER appuser
ENTRYPOINT ["java", "-jar", "app.jar"]
```

## Reference

- **Networking**: See [reference/networking.md](reference/networking.md)
- **Health checks & monitoring**: See [reference/monitoring.md](reference/monitoring.md)
