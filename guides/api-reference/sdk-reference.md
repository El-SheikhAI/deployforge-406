# DeployForge SDK Reference

The DeployForge SDK provides a programmatic interface to automate cloud deployments, manage infrastructure drift, and enable self-healing workflows. This reference covers core SDK components and usage examples.

## Installation

```bash
pip install deployforge-sdk
```

## Authentication

Authenticate using a service principal:

```python
from deployforge import DeployForgeClient

client = DeployForgeClient(
    api_key="your_api_key_here",
    tenant_id="your_tenant_id"
)
```

Or configure via environment variables:
```bash
export DEPLOYFORGE_API_KEY="your_api_key"
export DEPLOYFORGE_TENANT_ID="your_tenant_id"
```

## Core Concepts

### Deployments
Organize resources into logical units:

```yaml
# deployment.yaml
name: production-webapp
environments:
  - prod-us-east
  - prod-eu-central
resources:
  - type: kubernetes
    cluster: webapp-main
```

### Environments
Target specific cloud regions:

```python
env = client.environments.get("prod-us-east")
print(f"Cloud: {env.provider}, Region: {env.region}")
```

### Drift Detection
```python
scan = client.drift.start_scan(deployment_id="webapp-prod")
scan.wait_until_complete()
print(f"Drift detected: {scan.drift_found}")
```

### Self-Healing
Automatically repair infrastructure deviations:

```python
client.self_healing.enable(
    deployment_id="webapp-prod",
    auto_approve=True
)
```

## API Reference

### DeploymentClient Class

#### create_deployment(config: dict) → Deployment
```python
with open("deployment.yaml") as f:
    config = yaml.safe_load(f)
deployment = client.deployments.create(config)
```

#### get_deployment(deployment_id: str) → Deployment
```python
dep = client.deployments.get("webapp-prod")
```

#### list_deployments(filter: str=None) → list[Deployment]
```python
webapps = client.deployments.list(filter="name:webapp*")
```

#### update_deployment(deployment_id: str, patch: dict) → Deployment
```python
update = {"resources": [{"type": "kubernetes", "replicas": 5}]}
client.deployments.update("webapp-prod", update)
```

#### delete_deployment(deployment_id: str)
```python
client.deployments.delete("legacy-deployment")
```

### DriftDetectionClient Class

#### start_scan(deployment_id: str) → ScanJob
```python
scan = client.drift.start_scan("webapp-prod")
```

#### get_scan_results(scan_id: str) → ScanReport
```python
results = client.drift.get_scan_results("scan_12345")
print(results.drift_summary)
```

### SelfHealingClient Class

#### enable(deployment_id: str, auto_approve: bool=False)
```python
client.self_healing.enable("webapp-prod", auto_approve=True)
```

#### execute_repair(deployment_id: str)
```python
repair = client.self_healing.execute_repair("webapp-prod")
```

## Troubleshooting

**Common Errors:**
```python
try:
    deployment = client.deployments.get("missing-id")
except ResourceNotFoundError as e:
    print(f"Error: {e.message}")
```

**Debugging Tips:**
```python
client.enable_debug_logging()
client.deployments.list()  # Debug output to console
```

For additional examples and advanced usage, see the [DeployForge Developer Hub](https://docs.deployforge.com).