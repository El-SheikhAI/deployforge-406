# DeployForge Setup Guide

Welcome to DeployForge! This guide walks you through setting up our intelligent multi-cloud orchestrator. You’ll be deploying infrastructure across clouds with drift detection and self-healing in under 10 minutes.

## Prerequisites
- **Operating System**: Linux/macOS/Windows (WSL2 recommended for Windows)
- **Cloud Accounts**: AWS/Azure/GCP with admin permissions
- **Node.js v18+** & **npm v9+**
- **Terraform v1.5+**
- **Required Tools**: 
  ```bash
  git curl jq
  ```

## Step 1: Install DeployForge

### Option A: npm (Recommended)
```bash
npm install -g @deployforge/cli
```

### Option B: Standalone Binary
```bash
curl -sSL https://get.deployforge.io | bash -s -- -v 2.1.0
```

Verify installation:
```bash
deployforge version
# Should return: DeployForge CLI v2.1.0
```

## Step 2: Configure DeployForge

### Initialize Configuration
```bash
deployforge init
```
This creates `~/.deployforge/config.yaml` with commented examples.

### Configure Cloud Providers
Add credentials via **environment variables** (recommended) or config file:

```yaml
# AWS Example
providers:
  aws:
    access_key: ${AWS_ACCESS_KEY_ID}
    secret_key: ${AWS_SECRET_ACCESS_KEY}
    regions: [us-west-2, eu-central-1]

# Azure Example
  azure:
    tenant_id: "YOUR_TENANT_ID"
    client_id: "YOUR_CLIENT_ID"
    client_secret: ${AZURE_SECRET}
```

### Enable Drift Detection
Add to your config:
```yaml
auto_remediate: true
drift_check_schedule: "*/15 * * * *"  # Every 15 minutes
```

## Step 3: Deploy Your First Infrastructure

### Create Sample Project
```bash
mkdir my-first-deploy && cd my-first-deploy
deployforge blueprint create webserver --provider aws
```

### Deploy Infrastructure
```bash
deployforge deploy --blueprint webserver
```

Watch the orchestration:
```
[✓] Provisioning AWS VPC (vpc-0123456789) 
[✓] Configuring auto-healing web servers (3 nodes)
[✓] Drift detection enabled (15m intervals)
```

### Verify Deployment
```bash
deployforge status webserver
```

Access resources via the DeployForge Console:
```
https://console.deployforge.io/dashboard
```

## Next Steps
- [Explore CLI Commands](/references/cli-reference.md)
- [Configure Multi-Cloud Workloads](/guides/advanced/workload-distribution.md)
- [Customize Healing Policies](/guides/management/remediation-policies.md)

## Troubleshooting
**Installation Failed?**
```bash
export DFORGE_DEBUG=1
deployforge doctor  # Runs dependency checks
```

**Cloud Permission Issues**
```bash
deployforge validate config  # Checks credential validity
```

View execution logs:
```bash
tail -f ~/.deployforge/logs/main.log
```

Welcome to streamlined multi-cloud deployments! 🚀