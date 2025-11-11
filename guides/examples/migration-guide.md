# DeployForge Migration Guide

## Introduction  
Welcome to your smooth migration journey with DeployForge! This guide walks you through transitioning existing infrastructure deployments to DeployForge’s intelligent orchestration platform. Whether you’re migrating from Terraform, CloudFormation, or manual scripts, we’ve got you covered.

---

## Prerequisites
Before starting, ensure you have:
1. Working knowledge of your current IaC tool (Terraform, CloudFormation, etc.)
2. Access to your cloud provider credentials (AWS/Azure/GCP)
3. DeployForge CLI installed (`v2.1+`)
4. Existing infrastructure code available locally
5. Backup of current state files

---

## Migration Workflow

### 1. IaC Assessment  
Run the DeployForge Analyzer to scan your existing code:  
```bash
deployforge analyze --path ./my-infra
```
This generates:
- `migration_report.md`: Compatibility summary  
- `recommended_actions.yml`: Auto-generated fixes  

### 2. Update Cloud Providers  
Replace hardcoded provider blocks with DeployForge’s universal provider:  
```hcl
# BEFORE (Terraform)
provider "aws" {
  region = "us-east-1"
}

# AFTER 
provider "deployforge" {
  cloud  = "aws"
  region = "auto" # DeployForge selects optimal region
}
```

### 3. Configure Deployment Policies  
Create `deployforge.yml` in your project root:  
```yaml
policies:
  drift_detection:
    schedule: "0 4 * * *" # Daily checks
    auto_heal: true
  multi_cloud:
    strategy: "active-active" 
    failover_regions:
      - aws:eu-central-1
      - gcp:europe-west3
```

### 4. Refactor Resource Definitions  
Convert resources to DeployForge’s normalized syntax:  
```hcl
# BEFORE
resource "aws_s3_bucket" "data" {
  bucket = "legacy-data-bucket"
}

# AFTER
resource "deployforge:storage:object_bucket" "data" {
  name = "legacy-data-bucket"
  cloud_specific {
    aws.encryption = "AES256"
    gcp.location = "EU"
  }
}
```

### 5. Automated Validation  
Test your migration:  
```bash
deployforge validate --diff --plan
```

### 6. Guided Execution  
Execute migration via CLI:  
```bash
deployforge migrate --strategy=blue-green --downtime=5m
```
Monitor real-time progress in the dashboard:  
`https://deployforge.yourcompany.com/migrations/current`

### 7. Post-Migration Monitoring  
Enable self-healing:  
```bash
deployforge healing enable --resource=all
```

---

## Best Practices
1. **Modular Migration**  
   Migrate environments stage-by-stage (dev → staging → prod).

2. **State File Management**  
   Use DeployForge’s state transition tool:  
   ```bash
   deployforge state migrate --backend=s3 --lock-timeout=15m
   ```

3. **Pre-Migration Testing**  
   Validate non-production environments using:  
   ```bash
   deployforge test --scenario=full_destroy_rebuild
   ```

---

## Common Pitfalls & Solutions
| **Issue** | **Fix** |
|-----------|---------|
| State file locking conflicts | Use `--force-unlock` with migration UUID |
| Provider version mismatches | Run `deployforge deps sync` |
| Missing cloud-specific tags | Apply universal tagging policy |
| Permission drift | Enable IAM autopsy in dashboard |

---

## Troubleshooting  
**Error:** `Unsupported resource type`  
**Solution:**  
```bash
deployforge plugins install terraform/aws_v2_adapter
```

**Error:** `State version mismatch (v3 ≠ v4)`  
**Solution:**  
```bash
deployforge state upgrade --legacy-version=3
```

---

## FAQ  
**Q: Can I migrate while keeping legacy systems active?**  
A: Yes! Use our hybrid mode with `--hybrid-bridge=terraform_v12`.

**Q: How long do migration artifacts persist?**  
A: 30 days by default. Modify with `deployforge config retention --days=45`.

**Q: Does DeployForge support air-gapped environments?**  
A: Yes. Use our offline bundle generator via Enterprise Edition.

---

## Need Help?  
Contact our migration specialists:  
- Slack: `#deployforge-migrations`  
- Email: migrations@deployforge.com  
- Emergency Support: +1 (555) DEPL-OY1