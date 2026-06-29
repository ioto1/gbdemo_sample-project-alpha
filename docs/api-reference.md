# API Reference — Project Alpha

## Base URL

```
https://alpha.franka.de/api/v1
```

## Authentication

All endpoints require a Bearer token in the `Authorization` header:

```
Authorization: Bearer <your-token>
```

## Endpoints

### GET /robots

List all registered robots.

**Response 200:**
```json
{
  "robots": [
    {
      "id": "rbt-001",
      "name": "FR3-Arm-1",
      "status": "online",
      "ip": "172.16.0.101",
      "lastSeen": "2026-06-28T14:30:00Z"
    }
  ]
}
```

### POST /robots/:id/command

Send a command to a specific robot.

**Request:**
```json
{
  "command": "move_j",
  "params": {
    "joint_positions": [0.0, -0.785, 0.0, -2.356, 0.0, 1.571, 0.785]
  }
}
```

### GET /robots/:id/telemetry

Stream real-time telemetry via WebSocket.

```
ws://alpha.franka.de/api/v1/robots/rbt-001/telemetry
```

**Message format:**
```json
{
  "timestamp": "2026-06-28T14:30:01.123Z",
  "joint_positions": [0.01, -0.78, 0.02, -2.35, 0.01, 1.57, 0.79],
  "joint_torques": [0.5, 1.2, 0.3, 2.1, 0.4, 0.8, 0.6],
  "temperatures": [35.2, 36.1, 34.8, 37.3, 33.9, 35.5, 36.0]
}
```

## Error Codes

| Code | Meaning |
|------|---------|
| 400  | Bad request — check your payload |
| 401  | Unauthorised — invalid or expired token |
| 404  | Robot not found |
| 409  | Robot busy — another command in progress |
| 503  | Robot unreachable — check connectivity |
