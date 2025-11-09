# DeployForge API Types Reference

## Core Data Types
### Deployment
Represents a deployment configuration and state.

```typescript
interface Deployment {
  id: string;
  name: string;
  environmentId: string;
  blueprint: string;
  status: 'pending' | 'deploying' | 'active' | 'failed' | 'healing';
  createdAt: string; // ISO 8601 datetime
  updatedAt: string; // ISO 8601 datetime
  variables: Record<string, string | number | boolean>;
}
```

### Environment
Defines a deployment environment configuration.

```typescript
interface Environment {
  id: string;
  name: string;
  cloudProvider: 'aws' | 'azure' | 'gcp' | 'multi';
  regions: string[];
  driftDetectionEnabled: boolean;
  selfHealingEnabled: boolean;
  lastChecked?: string; // ISO 8601 datetime
  createdBy: string;
}
```

### Resource
Represents a cloud resource within a deployment.

```typescript
interface Resource {
  resourceId: string;
  providerType: string;
  resourceType: string;
  currentState: Record<string, any>;
  desiredState: Record<string, any>;
  lastDriftDetected?: string; // ISO 8601 datetime
  healthStatus: 'healthy' | 'degraded' | 'unavailable';
}
```

## Status Types
### DriftDetectionResult
Represents the outcome of a drift detection operation.

```typescript
interface DriftDetectionResult {
  deploymentId: string;
  detectionTime: string; // ISO 8601 datetime
  resourcesWithDrift: string[]; // Array of resource IDs
  totalResourcesChecked: number;
  requiresAction: boolean;
}
```

### HealingOperation
Represents a self-healing operation record.

```typescript
interface HealingOperation {
  operationId: string;
  triggerTime: string; // ISO 8601 datetime
  resolvedResourceIds: string[];
  status: 'completed' | 'failed' | 'in-progress';
  healingStrategy: 'auto-rollback' | 'auto-repair' | 'manual-review';
}
```

## Utility Types
### APIError
Standard error response structure.

```typescript
interface APIError {
  code: string;
  message: string;
  details?: Record<string, any>;
  timestamp: string; // ISO 8601 datetime
}
```

### Pagination
Pagination metadata for list responses.

```typescript
interface Pagination {
  totalItems: number;
  currentPage: number;
  pageSize: number;
  hasNextPage: boolean;
}
```

## Enumeration Types
### CloudProvider
```typescript
enum CloudProvider {
  AWS = 'aws',
  AZURE = 'azure',
  GCP = 'gcp',
  MULTI = 'multi'
}
```

### DriftSeverity
```typescript
enum DriftSeverity {
  NONE = 'none',
  LOW = 'low',
  MEDIUM = 'medium',
  HIGH = 'high',
  CRITICAL = 'critical'
}
```

---

**Note:** All date/time fields use ISO 8601 format (UTC): `YYYY-MM-DDTHH:mm:ss.sssZ`