# DeployForge Quickstart Guide

Welcome to DeployForge! This guide will help you deploy your first multi-cloud infrastructure in under 10 minutes using our intelligent orchestration platform.

## Before You Begin
### Prerequisites
- Docker v20.10+ installed
- Kubectl configured for cluster access
- DeployForge CLI installed ([see installation](#install-the-deployforge-cli))
- Access to at least one cloud provider account (AWS, GCP, Azure, or DigitalOcean)

## Install the DeployForge CLI

### macOS/Linux
```bash
curl -fsSL https://cli.deployforge.io/install.sh | bash
```

### Windows (PowerShell)
```powershell
irm https://cli.deployforge.io/install.ps1 | iex
```

Verify installation:
```bash
deployforge version
```

## Configure Your Environment

1. Create a configuration file:
```bash
mkdir -p ~/.deployforge
touch ~/.deployforge/config.yaml
```

2. Add your cloud credentials (example for AWS/GCP):
```yaml
# ~/.deployforge/config.yaml
providers:
  aws:
    access_key: "<YOUR_AWS_ACCESS_KEY>"
    secret_key: "<YOUR_AWS_SECRET_KEY>"
    region: "us-west-2"
  
  gcp:
    project_id: "<YOUR_GCP_PROJECT_ID>"
    credentials_json: "<PATH_TO_SERVICE_ACCOUNT_JSON>"
```

## Deploy Your First Infrastructure

1. Initialize a new project:
```bash
deployforge project init --name my-first-deployment \
  --providers aws,gcp \
  --scaffold vault,compute,database
```

2. Deploy to target environments:
```bash
cd my-first-deployment
deployforge deploy --env dev
```

DeployForge will:
- Provision multi-cloud resources
- Establish secure cross-cloud networking
- Configure automated drift detection
- Enable self-healing capabilities

## Verify Your Deployment

Check deployment status:
```bash
deployforge status --env dev
```

Access your Kubernetes cluster:
```bash
deployforge kubeconfig --env dev
kubectl get nodes
```

## Monitor and Manage

View real-time infrastructure health:
```bash
deployforge dashboard
```

Key features automatically enabled:
- Automatic drift correction
- Cross-cloud load balancing
- Security compliance scanning
- Cost optimization alerts

## Self-Healing Demo (Optional)

1. Simulate a resource failure:
```bash
deployforge chaos inject --env dev --service web-tier --type node-failure
```

2. Observe self-healing in action:
```bash
deployforge logs --env dev --service health-monitor
```

## Clean Up Resources

To avoid unexpected charges:
```bash
deployforge destroy --env dev --confirm
```

## Next Steps

Explore more advanced capabilities:
- [Multi-cluster Service Meshes](/guides/advanced/service-mesh)
- [GitOps Workflow Integration](/guides/integrations/gitops)
- [Custom Healing Policies](/guides/configuration/healing-policies)

Need help? Join our [Community Slack](https://slack.deployforge.io) or explore the [Full Documentation](/docs).