# Contributing to DeployForge

👋 Welcome, and thanks for your interest in improving DeployForge! Whether you’re fixing a typo or building a new feature, your contributions make our intelligent multi-cloud deployment orchestrator better for everyone.

## Before You Start

### Prerequisites
- Basic familiarity with Git/GitHub workflows
- [Git](https://git-scm.com/downloads) installed locally
- Valid AWS/Azure/GCP account credentials (for cloud provider integrations)
- [Terraform](https://www.terraform.io/downloads) v1.5+ (for infrastructure-as-code testing)
- [Kubernetes CLI](https://kubernetes.io/docs/tasks/tools/) (kubectl) for orchestration tests

For **development only** (additional requirements):
- Docker 20.10+
- Go 1.21+ or Python 3.10+ (language varies by component)
- GNU Make
- PostgreSQL 15 for local state tracking

## First-Time Setup

1. **Get the code**
   ```bash
   git clone https://github.com/yourorg/deployforge.git
   cd deployforge
   ```

2. **Install dependencies**
   ```bash
   make bootstrap  # Installs language-specific dependencies
   ```

3. **Configure environment**
   ```bash
   cp .env.example .env
   # Update values in .env with your cloud credentials
   ```

4. **Start development services**
   ```bash
   make dev-start  # Launches database and mock cloud services
   ```

## Contribution Workflow

### Step 1: Find an Issue
- Browse our [Good First Issues](https://github.com/yourorg/deployforge/labels/good%20first%20issue)
- Existing [Enhancements](https://github.com/yourorg/deployforge/labels/enhancement) and [Bugs](https://github.com/yourorg/deployforge/labels/bug)
- *For major features (>50 LOC)*: Open a Discussion first to align with our roadmap

### Step 2: Create a Branch
```bash
git checkout -b fix/button-styling  # Format: type/description
```
**Branch prefixes**:
- `feat/`: New functionality
- `fix/`: Bug corrections
- `docs/`: Documentation updates
- `refactor/`: Code improvements without behavior changes
- `test/`: Test-related changes

### Step 3: Make Changes
- Keep commits atomic: `git commit -m "fix(ui): Correct button alignment in dark mode"`
- Reference issues: `Closes #123` or `Relates to #456` in commit messages

### Step 4: Test Locally
```bash
make test-all      # Run unit & integration tests
make validate      # Check formatting and security scans
```

### Step 5: Open a Pull Request
1. Push to your fork: `git push origin your-branch-name`
2. Create PR against our `main` branch
3. **Required in all PRs**:
   - Passing CI checks
   - Updated documentation (if applicable)
   - Unit/Integration test coverage
   - Signed-off commits (`git commit -s`)

## Code Standards

### General Principles
- Infrastructure-as-code should be idempotent
- Cloud resources must support AWS/Azure/GCP parity
- Self-healing logic must include automatic rollback safeguards
- Always treat `main` as deployable production-ready state

### Language-Specific Rules
```markdown
| Component      | Linting Command          | Test Command            |
|----------------|--------------------------|-------------------------|
| Core Engine    | `make lint-go`           | `make test-go`          |
| Drift Detector | `make lint-py`           | `make test-py`          |
| UI Dashboard   | `make lint-js`           | `make test-cypress`     |
```

## Testing & Automation

### Test Categories
1. **Unit Tests**: Component isolation (`make test-unit`)
2. **Integration Tests**: Cloud provider interactions (`make test-integration`)
3. **End-to-End**: Full deployment lifecycle (`make test-e2e`)

**Every PR automatically runs:**
- Infrastructure drift detection test
- Cross-cloud compatibility matrix
- Performance benchmark comparison

## Documentation Expectations

1. Update relevant README sections
2. Annotate new configuration options in `docs/configuration-reference.md`
3. For visual changes: Add before/after screenshots using [GitHub’s image drag-and-drop](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#uploading-assets)
4. Use [Mermaid diagrams](https://mermaid.js.org/) for architecture changes:
   ```mermaid
   graph LR
     A[CI Pipeline] -->|Triggers| B(Orchestrator)
     B --> C{AWS}
     B --> D{Azure}
     B --> E{GCP}
   ```

## Security & Drift Reporting

**Never include in PRs:**
- Real cloud credentials
- Sensitive infrastructure IDs
- Private IP ranges/external hostnames

Found vulnerabilities? Report confidentially to security@yourorg.com.

## Governance Model

### Core Maintainers
- @joan-s (Tech Lead)
- @raju-k (Cloud Infrastructure)
- @morgan-d (CI/CD Systems)

### Decision Making
Significant changes require:
1. A Request for Comments (RFC) document (.rfcs/ folder)
2. 72-hour review period
3. Two maintainer approvals

---

This documentation last updated: Q3 2024. For real-time help, join our [Community Slack](https://deploy-forge.slack.com) or visit [Discussions](https://github.com/yourorg/deployforge/discussions).