# Hello World Tutorial

Welcome to your first DeployForge deployment! In this tutorial, you’ll deploy a simple `hello-world` web application to a cloud provider of your choice. By the end, you’ll understand how DeployForge automates provisioning, monitors for infrastructure drift, and maintains your desired state.

## Overview
- **Duration**: 10 minutes  
- **What You'll Do**:
  - Create a DeployForge project
  - Define a simple deployment configuration
  - Deploy a containerized app to a cloud provider
  - Simulate and resolve infrastructure drift automatically  

## Prerequisites
1. A [DeployForge account](https://app.deployforge.io/signup) (Free tier ok)
2. [DeployForge CLI](https://docs.deployforge.io/installation) installed
3. A configured cloud provider (AWS/Azure/GCP) in your account  

---

## Step 1: Create Your Project
```bash
mkdir hello-world
cd hello-world
touch deployforge.yml
```

## Step 2: Define Your Deployment
Add this to `deployforge.yml`:

```yaml
project: hello-world
environments:
  production:
    cloud: aws  # or azure/gcp
    region: us-west-2
    resources:
      web_app:
        type: container
        image: nginx:latest
        ports:
          - 80:80
        health_check:
          path: / 
          interval: 30s
```

**Key Features Demonstrated**:
- Self-healing: The `health_check` automatically restarts unhealthy containers
- Cloud-agnostic: Change `cloud` to `azure` or `gcp` without rewriting configs  

## Step 3: Deploy
```bash
# Authenticate CLI
deploy login

# Initiate deployment
deploy deploy

# Monitor progress
deploy status --live
```

Expected output:
```
✅ Deployment completed in 23s
Endpoint: http://hello-world-1234.prod.us-west-2.aws.deployforge.io
```

## Step 4: Verify Deployment
```bash
curl http://hello-world-1234.prod.us-west-2.aws.deployforge.io
```

You’ll see the default NGINX welcome page.

## Step 5: Simulate Drift Detection
1. **Manually break something** (using your cloud console):
   - Delete the deployed container
   - OR Change the exposed port from 80 to 8080
2. **Let DeployForge detect & repair**:
   ```bash
   deploy status --watch
   ```
   Within 2 minutes:
   ```
   🚨 Drift detected in resource 'web_app' (Type: container)
   🔄 Self-healing initiated: Recreating to match desired state
   ✅ Drift resolved in 37s
   ```

---

## Next Steps 🚀
You’ve just deployed an app with zero-touch recovery from outages and misconfigurations! Explore advanced features:
- [Multi-environment promotion](https://docs.deployforge.io/guides/environments)
- [Custom drift detection rules](https://docs.deployforge.io/guides/drift-detection)
- [Integrating Terraform/Helm](https://docs.deployforge.io/guides/integrations)

**Recommended tutorial**:  
➡️ [Deploy a Full-Stack App with CI/CD Pipeline](https://docs.deployforge.io/guides/full-stack-app)  

[![DeployForge Demo](https://img.shields.io/badge/LIVE_DEMO-DeployForge%20Dashboard-blue?logo=azurepipelines)](https://app.deployforge.io/demo)