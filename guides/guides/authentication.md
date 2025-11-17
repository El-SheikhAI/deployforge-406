# Authentication Guide

Secure authentication is essential for safe cloud operations. DeployForge supports multiple authentication methods across cloud providers and on-premises environments.

## Supported Methods
DeployForge integrates with modern authentication standards:
- **API Keys** (Cloud providers)
- **Service Accounts** (GCP, Azure)
- **OpenID Connect (OIDC)** (AWS, Azure)
- **Personal Access Tokens** (On-premises)
- **LDAP/SAML 2.0** (Enterprise systems)

## Cloud Provider Setup

### AWS Authentication
1. Create IAM user with `DeployForgeAdmin` policy
2. Configure AWS CLI profile:
```bash
aws configure --profile deployforge
```
3. DeployForge automatically detects standard credentials:
   - `~/.aws/credentials`
   - Instance profiles (for EC2 deployments)
   - Environment variables (`AWS_ACCESS_KEY_ID`/`AWS_SECRET_ACCESS_KEY`)

### Azure Authentication
1. Create service principal:
```bash
az ad sp create-for-rbac --name DeployForgePrincipal --role Contributor
```
2. Set environment variables:
```bash
export ARM_CLIENT_ID="<appId>"
export ARM_CLIENT_SECRET="<password>"
export ARM_TENANT_ID="<tenant>"
export ARM_SUBSCRIPTION_ID="<subscription>"
```
3. For OIDC authentication:
```hcl
provider "azuread" {
  oidc_request_token  = var.oidc_token
  oidc_request_url    = var.oidc_url
  oidc_token_file     = "/var/run/secrets/tokens/deployforge-token"
}
```

### Google Cloud Platform
1. Create service account with Deployment Manager permissions
2. Generate JSON key and set environment variable:
```bash
export GOOGLE_APPLICATION_CREDENTIALS="~/path/to/service-account.json"
```
3. For workload identity federation:
```yaml
authentication:
  gcp:
    workload_identity_provider: projects/123456789/locations/global/workloadIdentityPools/my-pool/providers/deployforge
    service_account: deployforge-runner@my-project.iam.gserviceaccount.com
```

## On-Premises Authentication
```bash
deployforge auth login --token <PERSONAL_ACCESS_TOKEN>

# For LDAP/AD integration:
auth_methods:
  ldap:
    server: "ldaps://ad.example.com"
    base_dn: "OU=users,DC=example,DC=com"
    username_attribute: "sAMAccountName"
```

## Advanced Configuration

### OIDC Authentication
Configure trust relationships in your cloud provider:

1. **AWS IAM Identity Provider**
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": {
      "Federated": "arn:aws:iam::ACCOUNT_ID:oidc-provider/deployforge.example.com"
    },
    "Action": "sts:AssumeRoleWithWebIdentity",
    "Condition": {
      "StringEquals": {
        "deployforge.example.com:aud": "deployforge-cluster"
      }
    }
  }]
}
```

### Secret Management Integration
```yaml
vault_integration:
  enabled: true
  provider: hashicorp_vault  # Supports AWS Secrets Manager, Azure Key Vault, GCP Secret Manager
  auth_method: approle
  config:
    role_id: "{{ env.VAULT_ROLE_ID }}"
    secret_id: "{{ env.VAULT_SECRET_ID }}"
```

## Security Best Practices
- 🔒 Rotate credentials every 90 days
- ⚠️ Never commit credentials to version control
- 🔑 Use temporary credentials where possible
- 👮‍♂️ Apply principle of least privilege
- 💻 Utilize hardware security modules (HSMs) for sensitive deployments

## Verification
Test your configuration:
```bash
deployforge auth test --provider aws
# Expected output: AWS credentials valid (expires in 1h15m)

deployforge auth list
# Shows all active authentication contexts
```

For enterprise SAML configuration, see our [Enterprise Integration Guide](../enterprise/saml-configuration.md).

> **Need Help?** Contact our [Support Team](mailto:support@deployforge.com) or join our `#authentication` channel in [Community Slack](https://slack.deployforge.com).