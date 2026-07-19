# 10 — Persistence Architecture

**Project:** GUARDIAN xMS
**Document type:** Platform architecture
**Status:** Draft
**Version:** 1.0

---

# 1. Purpose

This document defines the persistence architecture of GUARDIAN xMS.

The persistence layer shall preserve configuration, topology, telemetry, history and audit information while remaining independent of higher application logic.

---

# 2. Design Goals

The persistence architecture shall provide:

- correctness before performance
- temporal consistency
- immutable history where required
- explainability
- scalability
- backup friendliness
- technology independence

---

# 3. Data Domains

GUARDIAN xMS separates persistent information into logical domains.

```text
CMDB
Events
Time Series
Incidents
Recommendations
Automation
Audit
Configuration
Type Packages
```

Each domain has different access patterns and retention requirements.

---

# 4. Logical Storage Layers

```text
Applications
      │
      ▼
Repositories
      │
      ▼
Persistence Services
      │
      ├── CMDB Store
      ├── Time-Series Store
      ├── Event Store
      ├── Audit Store
      ├── Configuration Store
      └── Blob Store
```

The Core communicates only through repository interfaces.

---

# 5. CMDB Store

Stores:

- Sites
- Areas
- Locations
- Systems
- Assets
- Devices
- Components
- Relationships
- External Identities
- Type references

Characteristics:

- transactional
- strongly consistent
- referential integrity
- historized

---

# 6. Time-Series Store

Stores:

- sensor values
- calculated metrics
- health history
- communication statistics
- performance counters

Characteristics:

- append-oriented
- timestamp indexed
- retention policies
- aggregation support

---

# 7. Event Store

Stores immutable events.

Characteristics:

- append only
- replay capable
- chronological
- correlation friendly

Events are never modified after persistence.

---

# 8. Incident Store

Stores:

- incidents
- lifecycle
- acknowledgements
- root cause
- recommendations
- verification results

---

# 9. Audit Store

Stores:

- permission changes
- command execution
- approvals
- configuration changes
- lifecycle transitions
- administrative actions

Audit data is immutable.

---

# 10. Configuration Store

Stores:

- system configuration
- adapter configuration
- thresholds
- policies
- schedules
- notification settings

Configuration is versioned.

---

# 11. Blob Store

Stores:

- firmware
- reports
- attachments
- diagnostics
- exported files
- images
- backups

Metadata remains in the CMDB.

---

# 12. Repository Pattern

Repositories expose domain operations.

Example:

```text
DeviceRepository

find()
findByGid()
save()
archive()
history()
```

No business logic is implemented inside repositories.

---

# 13. Transactions

Typical atomic operations:

- Device replacement
- Relationship update
- Incident creation
- Permission changes
- Recommendation approval

---

# 14. Historical Data

History uses:

```text
valid_from
valid_until
```

Historical records remain queryable.

No silent overwriting.

---

# 15. Retention

Typical policies:

Events:
90–365 days

Telemetry:
configurable

Audit:
long-term

Incidents:
permanent

CMDB:
permanent

---

# 16. Backup Strategy

Backups include:

- configuration
- CMDB
- audit
- type packages
- time-series
- blob metadata

Recovery shall support point-in-time restoration where possible.

---

# 17. Performance

Optimisation techniques may include:

- indexing
- partitioning
- compression
- materialized views
- caching

Correctness has priority.

---

# 18. Technology Independence

Possible implementations include:

- PostgreSQL
- TimescaleDB
- SQLite
- object storage
- graph databases

No technology is mandated by this architecture.

---

# 19. Acceptance Criteria

The persistence architecture supports:

- immutable history
- temporal queries
- replay of events
- auditability
- scalable telemetry
- repository abstraction
- backup and recovery
- future technology migration

---

# 20. Related Documents

- 03 — Logical CMDB Data Model
- 06 — Event, Alarm and Incident Model
- 07 — Health Model
- 09 — Adapter and Integration Architecture
