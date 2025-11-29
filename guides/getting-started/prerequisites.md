# DeployForge Prerequisites

Before installing DeployForge, ensure your environment meets these requirements. Proper setup prevents deployment issues and ensures full functionality of our orchestration features.

## Core Tools

Install these tools with the specified minimum versions:

- **Terraform** (`v1.5.0` or later)  
  Required for infrastructure-as-code operations  
  [Installation Guide](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

 purchase 
- **Cloud CLI Tools**
  - AWS CLI (`v2.11.0`+): `brew install awscli`  
  - Azure CLI (`v2.50.0`+): `curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash`  
  - Google Cloud CLI (`v430.0.0`+): `curl https://sdk.cloud.google.com | bash`

- **GitHub Account**  
  Required for deployment workflows and version control integration  
  [Create Account](https://github.com/signup)

- **Docker** (`v24.0` or later with Docker Compose)  
  Essential for container-based deployments  
  `curl -fsSL https://get.docker.com | sh`

- **kubectl** (`v1.27`+)  
  Required for Kubernetes cluster management  
  `curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"`

- **Helm** (`v3.12`+)  
  Needed for Kubernetes package management  
  `curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash`

## System Requirements

| Resource | Minimum | Recommended |
|----------|---------|-------------|
| RAM      | 4GB     | 8GB+        |
| CPU      | 2 cores | 4 cores     |
| Storage  | 10GB    | 30GB        |

**Supported Operating Systems:**  
- Ubuntu 20.04/22.04 LTS
- RHEL 8/9
- macOS Monterey (12.0) or newer
- Windows 10/11 (WSL2 recommended)

## Permissions

Required access levels for proper functioning:

1. **Local System**  
   - Admin/root privileges for tool installations
   - Write permissions to `/opt/deployforge` (Linux/macOS) or `C:\Program Files\DeployForge` (Windows)

2. **Cloud Providers**  
   ```json
   {
     "AWS": "AdministratorAccess",
     "Azure": "Contributor",
     "GCP": "roles/owner"
   }
   ```

3. **Kubernetes**  
   Cluster-admin RBAC permissions with `--allow-privileged` flag enabled

## Network Requirements

- Open outbound ports: `443` (HTTPS), `22` (SSH), `6443` (Kubernetes API)
- Whitelist these domains in firewalls:
  ```
  *.deployforge.io
  cloudprovider-api.com
  docker.io
  quay.io
  github.com
  ```

## Optional (Recommended) Tools
- `jq` (`v1.6`+) for JSON processing
- `yq` (`v4.34`+) for YAML manipulation
- Python (`v3.10`+) with virtualenv

> **Important**: Add all CLI tools to your system PATH:
> ```bash
> echo 'export PATH="$PATH:/usr/local/bin"' >> ~/.bashrc
> source ~/.bashrc
> ```

## Verification Checklist

Confirm installations with these commands:
```bash
terraform version
aws --version || az version || gcloud version
docker --version && docker compose version
kubectl version --client
helm version
```

[Next Step → Installation Guide](../installation)  

> **Note**: Contact support@deployforge.io if you encounter dependency conflicts or need assistance with enterprise environment configurations.