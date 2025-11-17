# DeployForge Dashboard Guide

Welcome to the DeployForge Dashboard, your command center for intelligent cloud deployment management. This guide walks you through its key features and daily operations.

## Dashboard Overview
Access your dashboard by visiting `https://<your-deployforge-instance>/dashboard` after logging in. The interface includes:

- **Global Navigation Bar (Top)**: Access projects, environments, audit logs, and account settings
- **Activity Feed (Left Panel)**: Real-time deployment notifications
- **Main Workspace (Center)**: Interactive deployment maps and health metrics
- **Quick Actions (Right Panel)**: One-click access to common operations

---

## Deployment Monitoring

### Core Status Widgets
1. **Deployment Health Radar**:  
   ![Operational Status] Green ring = All systems nominal  
   ![Partially Degraded] Amber ring = 1+ components in warning state  
   ![Critical] Red ring = Immediate attention required

2. **Multi-Cloud Inventory**:  
   View resource counts across connected providers (AWS/Azure/GCP) in real-time:  
   ```plaintext
   AWS: 12 EC2 | 3 RDS | 8 S3  
   Azure: 4 VMs | 2 SQL  
   GCP: 6 Compute | 1 CloudSQL
   ```

---

## Drift Detection Center
Locate under **Governance → Drift Analysis**:
- **Time-slider**: Compare current state vs. 1h/24h/7d snapshots
- **Visual Diff**: Side-by-side infrastructure comparisons
  - ![Added Resource] Green highlight = New resources
  - ![Modified] Yellow highlight = Configuration changes
  - ![Missing] Red highlight = Unexpected deletions

Drift response options:
1. **Accept Changes** (Update baseline configuration)
2. **Restore from Snapshot** (Self-healing initiation)
3. **Schedule Remediation** (Delayed correction)

---

## Self-Healing Systems

### Automation Controls
![Self-Healing Toggle] Enable/disable per environment in **Settings → Automation**  
Three response modes:
1. **Aggressive** (Immediate correction without approval)
2. **Approval Required** (Pause for human validation)
3. **Report Only** (Log issues without action)

### Historical Recovery Data
View past remediation attempts with filters:
- **Success Rate Timeline** (30d/90d/All time)
- **Cost Impact** (Estimated recovery expenses)
- **Root Cause Tags** (e.g., `ConfigError`, `ProviderOutage`, `AccessViolation`)

---

## Activity Logs & Auditing
### Key Features:
- **Live Tail**: Stream events as they occur (`Ctrl+K` to open search)
- **Query Builder**:  
  ```sql
  type:deployment status:failed env:production 
  after:"2023-10-01"
  ```
- **Export Formats**: JSON, CSV, or PDF reports

---

## Customization
Personalize your dashboard via **User Profile → Preferences**:

1. **Widget Layout**: Drag-and-drop reorganization
2. **Threshold Alerts**:  
   `When cost variance > 15% → Send Slack#deploy-alerts`
3. **Theme Options**: Light/Dark/Custom CSS

---

## Support Resources
Need help? Use the diagnostic toolkit:
1. **Health Check**: `Dashboard Menu → Help → System Diagnostics`
2. **Troubleshooting Wizard**: Guided resolution for common issues
3. **Support Ticket**: Attach environment snapshots automatically

> **Tip**: Bookmark our [Keyboard Shortcuts Cheat Sheet](#) for faster navigation.

The DeployForge dashboard updates automatically every 30 seconds. Manual refresh available via ![Refresh Icon] in the top-right corner.