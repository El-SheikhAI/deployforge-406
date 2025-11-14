# Basic Usage Guide

Welcome to DeployForge! This guide walks you through deploying infrastructure using our intelligent orchestrator. 

## Prerequisites
- Installed DeployForge CLI (`deploy-forge` version 1.4+)
- Configured cloud provider credentials
- Initialized project (`deploy-forge init`)

## Sample Configuration
Create `deployment.yml`:

```yaml
project: my-web-app
environments:
  - name: production
    provider: aws
    region: us-west-2
    resources:
      - type: ec2-cluster
        count: 3
        instance: t3.medium
      - type: postgres-db
        version: 14
        storage: 100GB

self_healing:
  enabled: true
  schedule: hourly
```

## Deployment Command
Execute your deployment:

```bash
deploy-forge apply -f deployment.yml
```

DeployForge will:
1. Validate your configuration
2. Show execution preview
3. Request confirmation
4. Provision resources across specified clouds
5. Enable automated drift detection

## Key Operations

### Validate Configuration
```bash
deploy-forge validate -f deployment.yml
```

### Check Deployment Status
```bash
deploy-forge status production
```

### Dry-Run Simulation
```bash
deploy-forge plan -f deployment.yml
```

### Destroy Resources
```bash
deploy-forge destroy production
```

## Expected Output
Successful deployment shows:

```
✅ Deployment Complete (2m 18s)

Resources Provisioned:
• AWS EC2 Cluster (3 nodes) [us-west-2]
• Managed PostgreSQL 14 [100GB]

Active Monitoring:
✓ Automated drift detection enabled
✓ Self-healing policy activated (hourly checks)
```

## Next Steps
- Explore [advanced configurations](/guides/usage/advanced-config.md)
- Learn about [drift remediation](/guides/remediation/manual-drift.md)
- Set up [notifications](/guides/integrations/notifications.md)

Happy deploying! 🚀 DeployForge will automatically monitor your infrastructure and alert you to any configuration drift.