# Architecture — Project Alpha

## System Overview

![Architecture Diagram](assets/architecture.png)

Project Alpha follows a **microservices architecture** with three core services
and two supporting infrastructure components.

## Services

### alpha-web (React + TypeScript)

The frontend SPA served on port **8080**. Built with:
- React 18 + TypeScript
- MUI (Material UI v5) for component library
- React Query for server state management
- D3.js for real-time telemetry visualisation
- WebSocket client for live robot data

### alpha-api (Go 1.22)

REST + WebSocket API server on port **3001**. Built with:
- `chi` router for HTTP
- `gorilla/websocket` for telemetry streams
- Redis for job queue communication
- PostgreSQL for persistent state (robots, users, audit log)

### alpha-scheduler (Go 1.22)

Background job processor. Responsibilities:
- Consume jobs from Redis queue (`BSET` commands)
- Dispatch commands to robots via FCI (Franka Control Interface)
- Update job status in PostgreSQL
- Emit completion events via Redis pub/sub

## Data Flow

```
Browser ──HTTP/WSS──▶ alpha-web ──REST──▶ alpha-api ──Redis──▶ alpha-scheduler ──FCI──▶ Robot
                          │                    │
                          ▼                    ▼
                     PostgreSQL           PostgreSQL
```

## Infrastructure

| Component     | Technology     | Purpose |
|---------------|---------------|---------|
| Container     | Docker Compose | Local dev + CI |
| Orchestration | Kubernetes     | Production (EKS) |
| Message Queue | Redis 7        | Job dispatch |
| Database      | PostgreSQL 16  | Persistent state |
| Monitoring    | Prometheus + Grafana | Metrics and dashboards |

## Security

- mTLS between all services in production
- JWT-based authentication with 15-minute token expiry
- Robot credentials encrypted at rest (AES-256-GCM)
- Audit log for all robot commands with immutable storage
