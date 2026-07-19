# GUARDIAN xMS — Architecture Bundle (Documents 11–14)

---

# 11 — Service Architecture

## Purpose
Defines the logical service decomposition of GUARDIAN xMS.

### Core Services
- CMDB Service
- Event Service
- Alarm Service
- Incident Service
- Health Service
- Recommendation Service
- Automation Service
- Relationship Service
- Type Service
- Adapter Service
- Identity & Permission Service
- Audit Service
- Configuration Service
- Notification Service

### Principles
- Single Responsibility
- Stateless services where possible
- Event-driven collaboration
- Repository abstraction only
- No direct adapter coupling
- Explainability by design

### Service Interaction

```text
Adapters
   ↓
Event Service
   ↓
Alarm Service
   ↓
Incident Service
   ↓
Health Service
   ↓
Recommendation Service
   ↓
Automation Service
```

### Responsibilities
- **CMDB:** topology and assets
- **Event:** normalization and ingestion
- **Alarm:** rule evaluation and deduplication
- **Incident:** correlation and lifecycle
- **Health:** deterministic score calculation
- **Recommendation:** explainable actions
- **Automation:** controlled execution
- **Notification:** user communication
- **Audit:** immutable operational history

### Acceptance
Services are independently testable, loosely coupled and communicate only through contracts.

---

# 12 — API Architecture

## Purpose
Defines internal and external APIs.

### API Types
- REST API
- Event API
- WebSocket
- Internal Service API

### Design Principles
- Versioned
- Idempotent where applicable
- Resource-oriented
- Stable Guardian IDs
- Canonical data model

### Main Resources

```text
/sites
/systems
/assets
/devices
/components
/events
/alarms
/incidents
/health
/recommendations
/automation
/adapters
/type-packages
```

### Authentication
- OAuth2 / OIDC ready
- API Tokens
- Service Accounts

### Versioning

```text
/api/v1/...
```

Breaking changes require new API versions.

### Acceptance
Every public capability is accessible through documented APIs without exposing implementation details.

---

# 13 — Security Architecture

## Purpose
Defines the security model.

### Principles
- Zero Trust
- Least Privilege
- Defense in Depth
- Secure by Default
- Full Auditability

### Identity
- User
- Role
- Service Account
- Adapter Identity

### Authorization
- RBAC
- Time-limited permissions
- Object-level permissions
- Approval workflows

### Secrets
- Encrypted at rest
- Never stored in source code
- Rotatable
- Adapter isolation

### Audit
Every security-relevant action is logged:
- login
- permission changes
- automation
- approvals
- configuration changes

### Acceptance
Every privileged action is authenticated, authorized and auditable.

---

# 14 — Deployment Architecture

## Purpose
Defines deployment options.

### Target Platforms
- Local Server
- Docker
- Home Assistant Add-on
- Linux
- macOS (development)

### Logical Components

```text
Core
Adapters
Database
Time-Series
Audit
Web UI
API
```

### Deployment Principles
- Container-first
- Configuration as Code
- Rolling upgrades
- Health probes
- Backup before migration

### Home Assistant
GUARDIAN xMS may run as:
- standalone service
- Home Assistant Add-on

### Backup
Include:
- CMDB
- configuration
- audit
- type packages
- telemetry metadata

### Acceptance
Deployment is reproducible, upgradeable and recoverable with minimal downtime.

---

# Related Documents

01 Vision and Scope
02 Domain and CMDB Model
03 Logical CMDB Data Model
04 Type and Extension Model
05 Relationship Model
06 Event, Alarm and Incident Model
07 Health Model
08 Recommendation and Automation Model
09 Adapter and Integration Architecture
10 Persistence Architecture
