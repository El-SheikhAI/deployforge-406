# DeployForge API Authentication

Welcome to the DeployForge API authentication guide! Secure access is the cornerstone of our platform. This document details all supported authentication methods to interact with our API.

---

## Authentication Methods

DeployForge API supports three primary authentication mechanisms:

1. **Personal Access Tokens** (Recommended for automated scripts/clients)
2. **OAuth 2.0** (Recommended for third-party integrations)
3. **Session Cookie** (For browser-based interactions)

---

### 1. Personal Access Tokens (PAT)

Generate tokens through your DeployForge Dashboard for API access:

#### Creating a PAT
1. Navigate to **Settings > API Tokens**
2. Click "Generate Token"
3. Select appropriate scopes (e.g., `deployment:read`, `infrastructure:write`)
4. Store your token securely (it will only be shown once!)

#### Using the PAT
Include the token in the `Authorization` header:

```http
GET /api/v1/deployments HTTP/1.1
Host: api.deployforge.com
Authorization: Bearer dfpat_2Ld9zJ7sPq3NxWmYvr5t6g
```

---

### 2. OAuth 2.0 Authorization

Use this flow for third-party application integrations:

#### Step 1: Redirect for Authorization
```
GET https://auth.deployforge.com/oauth/authorize?
  response_type=code&
  client_id=YOUR_CLIENT_ID&
  redirect_uri=YOUR_REDIRECT_URI&
  scope=deployment:read%20infrastructure:write
```

#### Step 2: Exchange Code for Token
```http
POST /oauth/token HTTP/1.1
Host: auth.deployforge.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&
code=AUTH_CODE&
client_id=YOUR_CLIENT_ID&
client_secret=YOUR_CLIENT_SECRET&
redirect_uri=YOUR_REDIRECT_URI
```

Response:
```json
{
  "access_token": "df_at_Xd4k8vT3ZmFwLo5NY...",
  "token_type": "Bearer",
  "expires_in": 3600,
  "refresh_token": "df_rt_5Gz2pLmRqj9KbWn..."
}
```

#### Step 3: Use Access Token
Include in the `Authorization` header like PAT authentication.

---

### 3. Session Authentication

For browser-based interactions via the web interface:

1. Submit login credentials to `/auth/login`
```http
POST /auth/login HTTP/1.1
Host: app.deployforge.com
Content-Type: application/json

{
  "email": "user@company.com",
  "password": "your_secure_password"
}
```

2. Upon successful authentication:
   - Session cookie `df_session` will be set automatically
   - Include this cookie in subsequent requests

```http
GET /api/v1/projects HTTP/1.1
Host: app.deployforge.com
Cookie: df_session=7b7cd8e0-4a2f-4c7d...
```

---

## Error Handling

Common authentication-related responses:

| Code | Status          | Description                           |
|------|-----------------|---------------------------------------|
| 401  | Unauthorized    | Missing or invalid credentials        |
| 403  | Forbidden       | Valid credentials but insufficient scope |
| 429  | Too Many Requests | Rate limit exceeded (retry after header) |

Example error response:
```json
{
  "error": "invalid_token",
  "error_description": "Token expired at 2023-10-05T14:30:00Z"
}
```

---

## Security Best Practices

1. 🔒 Always use HTTPS for all API requests
2. ⏳ Rotate tokens regularly (90-day maximum)
3. 🔐 Store tokens using secure secret managers (never in source code)
4. 🛡 Apply principle of least privilege when granting scopes
5. 👁 Regularly review active tokens in your Dashboard
6. ✉️ Report compromised tokens immediately to security@deployforge.com

---

Next Steps:  
[API Rate Limits →](/guides/api-reference/rate-limiting)  
[Core API Endpoints →](/guides/api-reference/core-endpoints)