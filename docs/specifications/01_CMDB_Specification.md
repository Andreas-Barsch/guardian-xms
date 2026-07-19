# 01 — CMDB Specification

**Project:** GUARDIAN xMS  
**Status:** Draft  
**Version:** 1.0

---

# 1. Scope

This specification defines the Configuration Management Database (CMDB) of GUARDIAN xMS in sufficient detail to implement the first production version.

The CMDB is the authoritative source of truth for all managed objects.

---

# 2. Objectives

The CMDB shall:

- provide stable identities (Guardian IDs),
- separate functional Assets from physical Devices,
- preserve complete history,
- support topology and dependency analysis,
- support Health, Incident and Recommendation engines,
- remain vendor-neutral.

---

# 3. Bounded Context

The CMDB is responsible for:

- inventory
- topology
- identity
- relationships
- lifecycle
- historization

It is **not** responsible for:

- telemetry storage
- alarm processing
- automation
- health calculation

These consume CMDB information but do not own it.

---

# 4. Aggregate Roots

The following entities are aggregate roots:

- Site
- System
- Asset
- Device
- TypePackage

Components, Sensors and Actuators belong to their owning aggregate.

---

# 5. Invariants

Examples:

- Every object has exactly one immutable Guardian ID.
- Guardian IDs are never reused.
- Every active Device assignment is valid in time.
- Parent hierarchies are acyclic.
- Historical records are never overwritten.
- Archived objects cannot receive new active assignments.

---

# 6. Identity Model

Each managed object has:

```text
Technical ID
Guardian ID
External Identities
```

Guardian IDs remain stable for the lifetime of the object.

---

# 7. Lifecycle

Example lifecycle:

```text
planned
ordered
received
stored
installed
commissioning
active
maintenance
faulty
removed
retired
archived
```

Lifecycle transitions are validated and audited.

---

# 8. Historization

The CMDB uses temporal validity.

```text
valid_from
valid_until
```

Every historical query can reconstruct the topology at any point in time.

---

# 9. Repository Contracts

Example DeviceRepository:

```text
find()
findByGid()
save()
archive()
history()
findCurrentAssignment()
```

Repositories never contain business logic.

---

# 10. Business Rules

Examples:

- A Device may exist without an Asset.
- An Asset may temporarily exist without a Device.
- A Device replacement creates a new assignment.
- External identifiers are never primary keys.
- Relationships are first-class objects.

---

# 11. Validation

Validation includes:

- type compatibility
- temporal overlap
- hierarchy cycles
- mandatory relationships
- lifecycle transitions
- uniqueness constraints

---

# 12. Queries

Typical queries:

- current topology
- topology at time T
- locate Device by serial number
- locate Asset by Guardian ID
- find dependency graph
- find affected systems
- find assignment history

---

# 13. Example

Residential Site

```text
Site
 └── Energy System
      └── Battery Storage
            ├── Asset: Battery Position 1
            │      └── Device: US2000C #12345
            ├── Asset: Battery Position 2
            │      └── Device: US2000C #12346
            └── Asset: Battery Position 3
                   └── Device: US2000C #12347
```

Replacing Battery Position 2 changes only the Device assignment.

---

# 14. Non-functional Requirements

- deterministic behaviour
- complete auditability
- explainable state
- transaction safety
- testability
- technology independence

---

# 15. Acceptance Criteria

The implementation shall:

- reconstruct historical topology,
- support Asset/Device separation,
- preserve audit history,
- provide immutable identities,
- validate temporal consistency,
- support dependency traversal,
- expose repository interfaces for all aggregate roots.

---

# 16. Next Specifications

This specification is refined by:

- 02_Type_System_Specification.md
- 03_Event_Engine_Specification.md
- 04_Incident_Engine_Specification.md
- 05_Health_Engine_Specification.md
