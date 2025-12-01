# DeployForge Configuration Guide

Welcome to DeployForge! This guide will help you configure your intelligent deployment orchestrator. We recommend setting up your configuration files **before** initiating your first deployment.

## Configuration File Overview
DeployForge uses a declarative YAML configuration filed named **deployforge-config.yml** at your project’s root. This file defines your cloud environments, resources, policies, and automation rules.

```yaml
# deployforge-config.yml
version: 1.2
project: your-project-name
```

## Core Configuration Sections

### 1. Provider Setup
Connect your cloud accounts with provider blocks:

```yaml
providers:
  aws_prod:
    type: aws
    region: us-west-2
    auth_method: iam_role  # Alternatives: access_key, profile
    settings:
      assume_role_arn: arn:aws:iam::123456789012:role/DeployForgeProd

  gcp_staging:
    type: gcp
    project_id: my-staging-project
    auth_method: service_account
    credential_file: /secrets/gcp-service-account.json
```

### 2. Resource Definitions
Declare infrastructure components across multiple clouds:

```yaml
resources:
  - type: aws:s3_bucket
    name: customer_assets
    provider: aws_prod
    properties:
      bucket_name: acme-customer-assets
      acl: private
      versioning_enabled: true
      encryption: kms
      tags:
        environment: production

  - type: gcp:cloud_run
    name: api_service
    provider: gcp_staging
    properties:
      service_name: api-v2
      region: us-central1
      project: my-staging-project
      container_image: gcr.io/acme/api:v2.3
      concurrency: 100
```

### 3. Drift Detection Setup
Configure automated infrastructure monitoring:

```yaml
drift_detection:
  enabled: true
  schedule: "0 */2 * * *"  # Every 2 hours
  severity_levels:
    critical: [security_group, iam_policy]
    high: [instance_config, database_settings]
    medium: [tag_compliance]
```

### 4. Self-Healing Policies
Define automated remediation actions:

```yaml
self_healing:
  auto_remediate: true
  rules:
    - match:
        resource_types: [load_balancer, database]
        severity: medium
      action:
        name: auto-rollback
        params:
          version: previous_stable

    - match:
        resource_types: [cloud_function]
        severity: high
      action:
        name: redeploy-fresh
        max_attempts: 2
```

### 5. Notification Configuration
Stay informed about deployment changes:

```yaml
notifications:
  default_channel: ops_alerts
  integrations:
    email:
      recipients:
        - ops-team@acme.com
        - dev-leads@acme.com

    slack:
      webhook_url: ${SLACK_WEBHOOK}
      channels:
        - name: deploy_alerts
          notify_on: [drift, failure]
        - name: deploy_audit
          notify_on: [all]
```

## Validation and Testing

Verify your configuration with:

```bash
deployforge config validate --file deployforge-config.yml
```

## Next Steps
Your configured DeployForge can now:
1. Execute dry-run deployments (`deployforge plan`)
2. Commit config to version control
3. Establish deployment pipelines

> **Important Security Note**: Never commit credentials to source control. Use environment variables or secrets managers for authentication parameters.