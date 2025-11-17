# DeployForge API Response Codes  

This guide explains the HTTP response codes returned by the DeployForge API. All responses include a JSON payload with the following structure:  

```json
{
  "code": "HTTP_STATUS_CODE",
  "status": "success|error",
  "message": "Human-readable description",
  "data": {} // Optional context (e.g., validation errors, resource IDs)
}
```  

---

## Success Codes (2xx)  

### `200 OK`  
**When returned**:  
- Successful GET/PUT/PATCH requests  
- Drift detection results retrieval  

**Example**:  
```json
{
  "code": 200,
  "status": "success",
  "message": "Deployment configuration updated",
  "data": { "deployment_id": "dep-4xample" }
}
```  

---

### `201 Created`  
**When returned**:  
- Successful resource creation (e.g., deployment pipelines, environments)  

**Example**:  
```json
{
  "code": 201,
  "status": "success",
  "message": "Deployment pipeline created",
  "data": { 
    "pipeline_id": "pipe-4xample", 
    "location": "/pipelines/pipe-4xample" 
  }
}
```  

---

### `202 Accepted`  
**When returned**:  
- Async request accepted (e.g., long-running deployments, self-healing actions)  

**Example**:  
```json
{
  "code": 202,
  "status": "success",
  "message": "Deployment job started",
  "data": { 
    "job_id": "job-4xample", 
    "status_url": "/jobs/job-4xample/status" 
  }
}
```  

---

## Client Errors (4xx)  

### `400 Bad Request`  
**When returned**:  
- Malformed JSON  
- Missing required fields  
- Invalid parameter types  

**Example**:  
```json
{
  "code": 400,
  "status": "error",
  "message": "Missing 'cloud_provider' field in request body"
}
```  

---

### `401 Unauthorized`  
**When returned**:  
- Invalid/missing API key  
- Expired authentication token  

**Example**:  
```json
{
  "code": 401,
  "status": "error",
  "message": "Authentication credentials invalid"
}
```  

---

### `403 Forbidden`  
**When returned**:  
- User lacks permission for the requested resource/action  

**Example**:  
```json
{
  "code": 403,
  "status": "error",
  "message": "User cannot delete protected environments"
}
```  

---

### `404 Not Found`  
**When returned**:  
- Resource not found (e.g., invalid deployment ID)  
- Invalid API endpoint  

**Example**:  
```json
{
  "code": 404,
  "status": "error",
  "message": "Deployment 'dep-missing' not found"
}
```  

---

### `422 Unprocessable Entity`  
**When returned**:  
- Semantic validation errors (e.g., invalid cloud region, quota exceeded)  

**Example**:  
```json
{
  "code": 422,
  "status": "error",
  "message": "Validation failed",
  "data": {
    "errors": [
      { "field": "region", "message": "'us-east-99' is not a valid AWS region" }
    ]
  }
}
```  

---

## Server Errors (5xx)  

### `500 Internal Server Error`  
**When returned**:  
- Unhandled exceptions  
- Critical system failures  

**Example**:  
```json
{
  "code": 500,
  "status": "error",
  "message": "An unexpected error occurred. Our engineering team has been notified."
}
```  

---

### `503 Service Unavailable`  
**When returned**:  
- Planned maintenance windows  
- Cloud provider outages  

**Example**:  
```json
{
  "code": 503,
  "status": "error",
  "message": "DeployForge is undergoing maintenance. Check status.deployforge.com for updates."
}
```  

---

**Key Notes**:  
1. All responses use standard HTTP semantics.  
2. Response bodies **always** follow the `{ code, status, message, data }` structure.  
3. APIs are versioned; breaking changes include a major version bump (e.g., `/v2/...`).  
4. Include `Accept: application/json` in requests for JSON responses.  

Need help? Contact support@deployforge.com or visit our [API Status Dashboard](https://status.deployforge.com).  

```plaintext
Copyright © DeployForge. This documentation is licensed under CC BY 4.0.
```