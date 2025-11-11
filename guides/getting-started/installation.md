# DeployForge Installation Guide

Welcome to DeployForge! This guide walks you through installing our intelligent multi-cloud deployment orchestrator. Follow these steps to get started with automated drift detection and self-healing infrastructure.

---

## System Requirements
- **Operating Systems**  
  Linux (x86_64), macOS (10.15+), Windows 10/11 (WSL2 recommended)
- **Resources**  
  2+ CPU cores, 4GB RAM, 5GB disk space
- **Dependencies**  
  `curl`, `unzip`, Python 3.9+ (for CLI extensions)

---

## Install DeployForge

### **Linux (Debian/Ubuntu)**
```bash
curl -sSL https://install.deployforge.io/linux | sudo bash
sudo apt-get install deployforge-core -y
```

### **Linux (RHEL/CentOS)** 
```bash
curl -sSL https://install.deployforge.io/rhel | sudo bash
sudo yum install deployforge-core -y
```

### **macOS**
```bash
brew tap deployforge/tools
brew install deployforge
```

### **Windows (WSL2)**
1. Install [Windows Subsystem for Linux](https://aka.ms/wsl2)
2. Use Ubuntu installation commands above

---

## Verify Installation
Confirm successful installation with:
```bash
deployforge --version
# Expected output: DeployForge v1.0.0
```

---

## Quick Start Configuration
1. Initialize a project:
   ```bash
   deployforge init my-project
   cd my-project
   ```
2. Connect your cloud providers:
   ```bash
   deployforge cloud connect aws
   deployforge cloud connect azure
   ```
3. Deploy your first infrastructure:
   ```bash
   deployforge apply --auto-approve
   ```

---

## Next Steps
- Explore infrastructure templates in `/templates`
- Configure drift detection:
  ```bash
  deployforge monitor enable
  ```
- Join our community Slack at [slack.deployforge.io](https://slack.deployforge.io)

---

## Troubleshooting
**"Command not found" errors**  
Verify installation path is in your system PATH:
```bash
echo $PATH | grep '/usr/local/bin'
```

**Missing dependencies**  
Install required packages:
```bash
# Ubuntu/Debian
sudo apt-get install -y python3-dev git unzip

# RHEL/CentOS
sudo yum install -y python3-devel git unzip
```

**Cloud connection issues**  
Verify authentication credentials:
```bash
deployforge cloud status
```

---

Enjoy deploying with confidence! 🌐✨  
For advanced configurations, visit our [Documentation Hub](https://docs.deployforge.io).