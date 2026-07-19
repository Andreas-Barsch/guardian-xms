# 08 — Recommendation and Automation Model

**Project:** GUARDIAN xMS
**Document type:** Runtime architecture
**Status:** Draft
**Version:** 0.8

---

# 1. Purpose

This document defines how GUARDIAN xMS transforms Incidents into explainable Recommendations and, where permitted, into safe Automation.

Core principle:

> Recommendation first — Automation second.

Automation is never the default. The system should first explain *what* it recommends, *why* it recommends it and *what the expected outcome is*.

---

# 2. Processing Flow

```text
Telemetry
    ↓
Events
    ↓
Alarms
    ↓
Incidents
    ↓
Root Cause
    ↓
Recommendation Engine
    ↓
Decision
    ├── User approval
    └── Automation Policy
            ↓
Execution
            ↓
Verification
            ↓
Audit
```

---

# 3. Recommendation

A Recommendation is an explainable proposal for action.

Logical fields:

```text
recommendation_id
gid
incident_id
priority
title
description
reasoning
expected_effect
estimated_risk
estimated_duration
confidence
created_at
status
```

Status:

- proposed
- accepted
- rejected
- expired
- executed

---

# 4. Prioritisation

Priority is derived from:

- Incident severity
- business criticality
- affected topology
- safety impact
- outage duration
- confidence
- available redundancy

Suggested levels:

- Critical
- High
- Medium
- Low
- Informational

---

# 5. Recommendation Categories

Examples:

- Inspect
- Measure
- Replace
- Reconnect
- Restart
- Update Firmware
- Change Configuration
- Contact Vendor
- Observe
- Ignore (false positive)

---

# 6. Automation Policies

Policies determine whether execution is allowed.

Typical modes:

```text
Manual
Recommendation Only
Approval Required
Automatic
Emergency Automatic
```

Policies may differ by object type and safety class.

---

# 7. Safety Evaluation

Before execution the engine evaluates:

- permissions
- maintenance windows
- interlocks
- dependencies
- redundant paths
- actuator availability
- rollback availability
- confidence threshold

Failure of any mandatory check prevents execution.

---

# 8. Approval Workflow

Approval may require:

- operator
- administrator
- multiple approvals
- time window
- emergency override

Every approval is audited.

---

# 9. Verification

After execution GUARDIAN xMS verifies:

- command success
- expected state transition
- disappearance of alarms
- improvement of health
- absence of new incidents

If verification fails, follow-up recommendations may be generated.

---

# 10. Rollback

Where supported, automation defines rollback procedures.

Examples:

- restore previous operating mode
- reconnect relay
- cancel configuration change

Rollback capability is part of the CommandDefinition.

---

# 11. Home Assistant Integration

Recommendations may appear as:

- dashboard cards
- persistent notifications
- mobile push notifications
- approval dialogs

Approved actions may invoke Home Assistant services or adapter commands.

---

# 12. Audit

Every decision records:

- recommendation
- approver
- policy
- executed command
- verification result
- timestamps
- correlation ID

---

# 13. Acceptance Criteria

The model supports:

- explainable recommendations
- configurable automation policies
- approval workflows
- verification after execution
- rollback support
- complete auditability
- vendor-neutral execution
