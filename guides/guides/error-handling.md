# DeployForge Error Handling Guide

DeployForge's error handling system is designed to give you clear insights when things go wrong—and equip you with the tools to resolve issues quickly. This guide covers recognizing, troubleshooting, and resolving common deployment errors within our platform.

## Understanding Deployment Errors

Errors fall into four main categories:
- **Deployment Errors**: Failures during infrastructure provisioning/modification
- **Configuration Errors**: Invalid settings in your deployment manifests
- **Drift Detection**: Infrastructure diverges from declared state
- **Self-Healing Actions**: Automated recovery attempts (success/failure reporting)

## Identifying Errors

### Where to Look
1. **Deployment Dashboard**  
   Real-time status indicators in the web UI with color-coded severity:
   - ![Critical](https://img.shields.io/badge/-Critical-red)
   - ![Warning](https://img.shields.io/badge/-Warning-yellow)
   - ![Recovered](https://img.shields.io/badge/-Recovered-green)

2. **Command Line Feedback**  
   Immediate error messages when running `deployforge apply`:
   ```bash
   ERROR: Resource allocation timeout (cloud_provider: aws)
   Suggestion: Validate quota limits in us-east-1
   ```

3. **Log Explorer**  
   Access detailed logs via:
   ```bash
   deployforge logs --deployment <deployment_id> --tail
   ```

![DeployForge Error Log Structure](https://example.com/path/to/error-log-screenshot.png)

## Common Errors & Solutions

### 1. Timeout Errors
**Symptoms**  
`Timeout waiting for resource provisioning (300s)`  
`Cloud API rate limit exceeded`

**Fix Checklist**:
- Verify cloud provider quota limits
- Increase timeout in your deploy manifest:
  ```yaml
  resources:
    my_database:
      type: aws/rds
      timeout: 600 # Increase to 10 minutes
  ```
- Enable automatic retries:
  ```yaml
  policies:
    retries: 3
  ```

### 2. Dependency Conflicts
**Symptoms**  
`Circular dependency detected between resource_a and resource_b`  
`Missing required output from resource_c`

**Resolution Steps**:
1. Run dependency analysis:
   ```bash
   deployforge validate --deps
   ```
2. Review the dependency graph in web UI
3. Add explicit dependencies in your manifest:
   ```yaml
   resources:
     resource_b:
       depends_on: [resource_a]
   ```

### 3. Configuration Drift
**Symptoms**  
`DRIFT DETECTED: Security group rules changed externally`  
`Manual modification detected in resource: web_server`

**Self-Healing Options**:
- Accept changes (update your manifest to match actual state):
  ```bash
  deployforge reconcile --accept
  ```
- Revert to declared state:
  ```bash
  deployforge repair --resource web_server
  ```

### 4. Permission Issues
**Symptoms**  
`AccessDeniedException when calling CreateRole`  
`Insufficient IAM permissions for resource creation`

**Quick Checks**:
```bash
# Validate credentials
deployforge auth test

# Verify required permissions
deployforge requirements --cloud aws
```

## Advanced Troubleshooting

### Debug Mode
Enable verbose logging:
```bash
deployforge apply --log-level=debug
```

### State Inspection
Examine current deployment state:
```bash
deployforge state inspect <deployment_id> --format=json
```

### Manual Intervention
When automatic recovery fails:
1. Pause automation:
   ```bash
   deployforge pause --deployment <id>
   ```
2. Perform manual fixes via cloud console
3. Resume synchronization:
   ```bash
   deployforge resume --deployment <id>
   ```

## FAQ

**Q: How long does drift detection take?**  
A: Periodic checks run every 30 minutes. Instant checks occur on `deployforge plan` executions.

**Q: Can I customize error notifications?**  
A: Yes! Configure alert channels in `deployforge.yml`:
```yaml
notifications:
  slack:
    error_channel: "#deploy-alerts"
  pagerduty:
    critical_events: true
```

**Q: Where are logs stored?**  
A: By default:
- 7 days in DeployForge cloud storage
- Your cloud provider's native logging (e.g., AWS CloudWatch)

---

Need more help? Join our [community forum](https://forum.deployforge.example.com) or contact support through the web UI's help menu.

Happy deploying! 🚀  
*The DeployForge Team*