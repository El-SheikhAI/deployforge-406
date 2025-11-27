# DeployForge Data Model: Core Concepts

At the heart of DeployForge lies a robust data model that orchestrates multi-cloud deployments while enabling automated drift detection and self-healing capabilities. Understanding these foundational elements will help you design more effective infrastructure patterns.

## Key Entities

### 1. Deployment
The primary unit of execution representing a complete infrastructure deployment. 

Attributes include:
- **ID**: Unique deployment identifier (UUIDv4)
- **Target Environment**: Reference to an Environment entity
- **Desired State**: Infrastructure-as-Code (IaC) configuration
- **Execution Plan**: Step-by-step resource orchestration logic
- **Status**: Current lifecycle stage (`pending`, `deployed`, `failed`, `healing`)

### 2. Resource
Represents actual infrastructure components across cloud providers.

Characteristics:
- **Resource ID**: Cloud-native identifier (e.g., AWS ARN, Azure Resource ID)
- **Provider**: Cloud platform (AWS/Azure/GCP/OVH/etc.)
- **Current Configuration**: Last observed state
- **Dependencies**: Other resources required for operation
- **Health Status**: Operational state indicator

### 3. State Snapshot
Point-in-time representation of infrastructure state critical for drift detection.

Contains:
- **Timestamp**: Unix epoch of capture
- **Resource States**: Key/value pairs of configuration attributes
- **Drift Score**: Percentage deviation from desired state
- **Signature**: Cryptographic hash of snapshot contents

### 4. Policy
Rule set governing deployment behavior and healing operations.

Key elements:
- **Drift Thresholds**: Allowable deviation before triggering alerts/healing
- **Healing Actions**: Automated response sequences (reboot/recreate/scale)
- **Approval Gates**: Manual intervention requirements
- **Compliance Rules**: Security/configuration standards

### 5. Environment
Logical grouping of resources with shared characteristics.

Includes:
- **Cloud Targets**: List of provider accounts/regions
- **Network Topology**: Connectivity specifications
- **Config Templates**: Shared variables/secrets
- **Access Policies**: User/Role permissions matrix

## Entity Relationships

```text
┌─────────────┐       ┌─────────────┐       ┌───────────────┐
│ Deployment  │──────▶│  Resource   │──────▶│ State Snapshot│
└─────────────┘       └─────────────┘       └───────────────┘
      │                       ▲                     │
      ▼                       │                     ▼
┌─────────────┐       ┌─────────────┐       ┌─────────────┐
│ Environment │◄──────│   Policy    │◄──────│  Drift Alert │
└─────────────┘       └─────────────┘       └─────────────┘
```

### Example Scenario
1. A **Deployment** targets an **Environment** spanning AWS and Azure
2. Creates **Resources** from IaC definitions with terraform/cloudformation
3. Continuous **State Snapshots** detect configuration drift exceeding **Policy** thresholds
4. Triggers **Healing Actions** to restore desired state without human intervention

## Data Flow Overview

1. **Declaration Phase**:  
   Deployment + IaC → Environment mapping → Policy application

2. **Orchestration Phase**:  
   Resource provisioning → State Snapshot (baseline)

3. **Monitoring Phase**:  
   Current State ↔ Baseline State → Drift scoring

4. **Remediation Phase**:  
   Policy evaluation → Healing execution → New baseline creation

This model enables DeployForge to maintain infrastructure integrity across heterogeneous cloud environments while providing audit trails of all state transitions.