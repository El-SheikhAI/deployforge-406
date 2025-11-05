# DeployForge API Endpoints Guide

## Overview
This document provides detailed information about DeployForge's REST API endpoints. All endpoints require `Bearer Token` authentication and return JSON responses.

---

## Authentication
All requests require an `Authorization` header with a valid API token:
```http
Authorization: Bearer <your_api_token>
```

---

## Base URL
`https://api.deployforge.com/v1`

---

## Environments

### List Environments
```http
GET /organizations/{organization_id}/environments
```
**Parameters**
- `organization_id` (string, path): Organization identifier  
- `type` (string, query optional): Filter by environment type (aws/gcp/azure)  
- `status` (string, query optional): Filter by status (active/inactive)  

**Example Response**
```json
{
  "data": [
    {
      "id": "env_2XyB8NzLp9",
      "name": "prod-us-west",
      "provider": "aws",
      "created_at": "2023-09-15T14:30:00Z"
    }
  ],
  "pagination": {
    "next_page": "2",
    "total": 42
  }
}
```

---

### Create Environment
```http
POST /organizations/{organization_id}/environments
```
**Parameters**
- `organization_id` (string, path)

**Request Body**
```json
{
  "name": "staging-europe",
  "provider": "gcp",
  "configuration": {
    "region": "eu-central1",
    "credentials": "<encrypted_secret>"
  }
}
```

**Success Response**  
`201 Created`
```json
{
  "id": "env_5TmRqKs3yD",
  "activation_url": "/environments/env_5TmRqKs3yD/initialize"
}
```

---

## Deployments

### Start Deployment
```http
POST /environments/{environment_id}/deployments
```
**Request Body**
```json
{
  "manifest_url": "https://git.company.com/repo/deploy.yml",
  "strategy": "blue-green",
  "notify_channels": ["slack", "email"]
}
```

**Response**
```json
{
  "deployment_id": "deply_8HbN3jK7fG",
  "status": "initializing",
  "logs_url": "/deployments/deply_8HbN3jK7fG/logs"
}
```

---

### Get Deployment Status
```http
GET /deployments/{deployment_id}
```

**Response**
```json
{
  "id": "deply_8HbN3jK7fG",
  "current_state": "running",
  "progress": 65,
  "estimated_completion": "2023-11-22T09:45:00Z"
}
```

---

## Drift Detection

### Trigger Drift Scan
```http
POST /environments/{environment_id}/drift-scans
```
**Response**
```json
{
  "scan_id": "scan_9RtP2zLq4X",
  "initiated_by": "user_1234",
  "started_at": "2023-11-20T14:22:11Z"
}
```

---

### Get Scan Results
```http
GET /drift-scans/{scan_id}
```

**Response**
```json
{
  "id": "scan_9RtP2zLq4X",
  "status": "completed",
  "drifts_detected": 3,
  "critical_findings": [
    {
      "resource_id": "sg-0238a8ef1f6dbb48f",
      "expected_config": "...",
      "actual_config": "..."
    }
  ]
}
```

---

## Error Responses
```json
{
  "error": {
    "code": "ERR-401",
    "message": "Invalid authorization token",
    "documentation_url": "https://docs.deployforge.com/errors/ERR-401"
  }
}
```
**Common Error Codes:**
- `400` Bad Request
- `401` Unauthorized
- `403` Forbidden
- `404` Resource Not Found
- `429` Rate Limited
- `500` Internal Server Error

---

## Rate Limits
- `100 requests/minute` per API token  
- `5 concurrent deployments` per organization  
- Headers returned with every response:
  ```
  X-RateLimit-Limit: 100
  X-RateLimit-Remaining: 97
  ```