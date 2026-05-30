# LaookProj

Welcome to the central repository for **LaookProj** – an AI-powered pantry tracker and recipe recommendation engine driven by Computer Vision.

This repository serves as the central hub for organization-wide governance, documentation, security policy, and repository standards.

---

## 🏛️ Organization Structure

LaookProj is organized into several dedicated repositories:

- [laook-backend-api](https://github.com/LaookProj/laook-backend-api): The backend service, API, and core logic.
- [laook-models](https://github.com/LaookProj/laook-models): Machine learning and Computer Vision models for pantry items tracking.
- [laook-mobile-app](https://github.com/LaookProj/laook-mobile-app): The primary native mobile application.
- [laook-flutter-app](https://github.com/LaookProj/laook-flutter-app): The cross-platform mobile application.

---

## 🛡️ Governance & Security Policies

All repositories in the `LaookProj` organization are governed by strict branch protection policies and role-based access control (RBAC).

### 👥 Role-Based Access (RBAC)
- **Developers Team (`@LaookProj/developers`)**: All active engineers have write access through this team. Direct commit access is blocked.
- **QA Team (`@LaookProj/qa`)**: Responsible for quality assurance, automated test verification, and release validation.

### 🔒 Branch Protection (Default Branch: `main`)
All repositories enforce standard protection on their default branch:
- **1 Approving Review Required**: Pull Requests must be reviewed and approved. Stale approvals are automatically dismissed on new commits.
- **Strict Status Checks**: PR builds must pass `ci/build` and `ci/lint` before merging.
- **Commit Signatures**: All commits must possess valid GPG or SSH signatures.
- **Linear History**: Only squash merging is enabled to keep the git history clean and readable.
- **Enforced Admins**: Rules apply to administrators and owners as well.

The stable branch protection policy JSON used to configure all organization repositories is located at [branch_protection.json](./branch_protection.json).

---

## 📞 Security & Vulnerability Reporting

If you find a security vulnerability, please do not report it publicly via GitHub issues. Please refer to our [SECURITY.md](./SECURITY.md) for disclosure guidelines and contact information.
