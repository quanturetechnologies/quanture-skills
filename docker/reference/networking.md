# Docker Networking Reference

## Custom network (isolate services)

```yaml
services:
  app:
    networks:
      - frontend
      - backend

  db:
    networks:
      - backend   # db not exposed to frontend

  nginx:
    networks:
      - frontend
    ports:
      - "80:80"   # only nginx exposed

networks:
  frontend:
  backend:
    internal: true  # no external access
```

## Service discovery

Services communicate using service name as hostname:
```bash
# from 'app' container, reach 'db' container:
DB_HOST=db
```

## Expose vs ports

```yaml
# ports: exposes to HOST machine
ports:
  - "8000:8000"

# expose: only accessible to other containers (no host access)
expose:
  - "8000"
```

Use `expose` for internal services (db, cache). Use `ports` only for public endpoints.
