# DeployForge Changelog

All notable changes to the DeployForge project are documented in this file.  
This project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]
### Added
- GitHub Actions integration for automated changelog validation

## [1.1.0] - 2023-10-17
### Added
- Azure Kubernetes Service (AKS) deployment target support
- Environment variable management in web dashboard
- `deployforge export-config` CLI command

### Changed
- Improved drift detection heuristics by 30% for AWS resources
- Web dashboard loading performance improved by 45%

### Fixed
- Graceful handling of expired cloud provider credentials
- CLI configuration file parsing edge cases

## [1.0.0] - 2023-08-25
### Added
- Multi-cloud deployments (AWS, GCP, DigitalOcean initial support)
- Automated drift detection system
- Self-healing infrastructure module
- Command-line interface (CLI) tool
- Web dashboard for deployment visualization

### Changed
 Lcommented out previous ALPHA headers per stable release
- Refactored core orchestration engine for extensibility

### Security
- Implemented automatic secret rotation mechanism
- HTTPS enforced for all API endpoints

---

*Changelog format inspired by [Keep a Changelog](https://keepachangelog.com/en/1.1.0/)*