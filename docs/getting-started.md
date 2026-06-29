# Getting Started — Project Alpha

## Prerequisites

- Docker 24+ and docker-compose 2+
- Node.js 20+ (for local development)
- Access to a Franka robot or the simulator (FCI)

## Installation

### 1. Clone the repository

```bash
git clone https://gitlab.franka.de/franka-dev/cloud/project-alpha.git
cd project-alpha
```

### 2. Configure environment

```bash
cp .env.example .env
# Edit .env — set ROBOT_API_URL and AUTH_TOKEN
```

### 3. Start services

```bash
docker-compose up -d
```

Verify with `docker-compose ps` — all three services should be `healthy`.

### 4. Connect your first robot

Open `http://localhost:8080` → **Add Robot** → enter IP and FCI credentials.

## Development

```bash
# Frontend only
cd web && npm install && npm run dev

# API only
cd api && go run ./cmd/server

# Run tests
docker-compose -f docker-compose.test.yml up --abort-on-container-exit
```
