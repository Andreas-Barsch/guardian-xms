# 05 — Relationship and Dependency Model

**Project:** GUARDIAN xMS
**Document type:** Relationship architecture
**Status:** Draft
**Version:** 0.5

---

## 1. Purpose

Relationships are first-class CMDB objects.

They describe structural, logical and operational dependencies independently from the object hierarchy.

---

## 2. Principles

- Relationships are historized.
- Relationships have their own Guardian ID.
- Relationships carry provenance and confidence.
- Relationships are version-independent.
- Relationships are never inferred implicitly in the core.

---

## 3. Relationship Categories

### Structural

- contains
- located_at
- installed_in
- assigned_to
- occupies_position

### Supply

- supplies
- feeds_into

### Communication

- communicates_with
- connected_to_bus
- reports_to

### Control

- controls
- controlled_by

### Monitoring

- monitors
- measures

### Dependency

- depends_on
- impacts
- caused_by

### Resilience

- redundant_with
- backs_up
- protected_by
- isolated_by

---

## 4. Logical Model

```text
Relationship
------------
id
gid
relationship_type_id
source_type
source_id
target_type
target_id
direction
status
criticality
confidence
provenance
valid_from
valid_until
attributes_json
created_at
updated_at
```

---

## 5. Temporal Rules

- Validity interval: [valid_from, valid_until)
- valid_until = null means active.
- Historical relationships are never overwritten.
- Exclusive relationship intervals must not overlap.

---

## 6. Dependency Types

- hard dependency
- soft dependency
- conditional dependency
- performance dependency
- informational dependency

---

## 7. Root Cause

Root cause analysis traverses relationships such as:

- depends_on
- communicates_with
- supplies
- reports_to
- caused_by

---

## 8. Impact Analysis

Impact calculation considers:

- dependency chains
- redundancy
- protection
- communication paths
- criticality

---

## 9. Health Integration

Relationships influence:

- Device Health
- Category Health
- Overall Health

Missing redundancy or broken communication paths reduce confidence and health.

---

## 10. Automation

Automation rules evaluate:

- dependencies
- permissions
- protection
- interlocks

before actuator commands are executed.

---

## 11. Example

```text
Residential Energy System
    ├── Hycube Inverter
    ├── Battery Stack
    │      ├── Battery Position 1
    │      ├── Battery Position 2
    │      └── ...
    └── Home Assistant

Relationships

Battery -> communicates_with -> Home Assistant
Battery -> reports_to -> Hycube
Battery -> depends_on -> BMS
Home Assistant -> monitors -> Battery
```

---

## 12. Validation Rules

1. No invalid self references.
2. Hierarchies must remain acyclic.
3. Type compatibility is mandatory.
4. Invalid time overlaps are rejected.
5. Relationship provenance is required for discovered links.
6. Critical control paths require permissions.
7. Archived objects cannot receive new active relationships.

---

## 13. Consequences

The graph enables:

- topology views
- impact analysis
- root cause analysis
- incident correlation
- health calculation
- recommendation generation
- automation planning

---

## 14. Related ADRs

- ADR-004 — Relationships as First-Class CMDB Objects
