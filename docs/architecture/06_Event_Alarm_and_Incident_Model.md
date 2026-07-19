# 06 — Event, Alarm and Incident Model

**Project:** GUARDIAN xMS  
**Document type:** Runtime architecture  
**Status:** Draft  
**Version:** 0.6

---

# 1. Purpose

This document defines the runtime processing model for Events, Alarms and Incidents.

It specifies how raw observations become actionable operational knowledge.

The model is vendor-neutral and independent from transport protocols.

---

# 2. Processing Pipeline

```text
Telemetry
    ↓
Normalization
    ↓
Event
    ↓
Classification
    ↓
Alarm
    ↓
Correlation
    ↓
Incident
    ↓
Recommendation
    ↓
Automation (optional)
```

---

# 3. Event

An Event represents a single observable fact.

Examples

- sensor value changed
- device connected
- battery communication lost
- voltage below threshold
- firmware updated
- command executed

Logical attributes

```text
event_id
gid
occurred_at
received_at
source
event_type
severity
object_type
object_id
metric
value
unit
quality
correlation_key
payload
```

Events are immutable.

---

# 4. Event Severity

Levels:

- Trace
- Debug
- Information
- Notice
- Warning
- Error
- Critical

Severity describes the event itself, not necessarily the business impact.

---

# 5. Alarm

An Alarm is an evaluated operational condition.

Alarm lifecycle:

```text
New
↓
Active
↓
Acknowledged
↓
Resolved
↓
Closed
```

An Alarm always references one or more Events.

---

# 6. Alarm Deduplication

Repeated identical events shall not create unlimited alarms.

Deduplication keys may include:

- object
- metric
- alarm rule
- time window

---

# 7. Incident

An Incident represents one operational problem.

Multiple alarms may belong to the same Incident.

Examples

- Battery communication failure
- Inverter unavailable
- Home Assistant offline
- Grid meter failure

---

# 8. Incident Lifecycle

```text
Detected
↓
Confirmed
↓
Investigating
↓
Mitigated
↓
Resolved
↓
Closed
```

Incidents may be reopened when new evidence appears.

---

# 9. Correlation

Correlation considers:

- topology
- dependencies
- communication paths
- time proximity
- recurring patterns
- common root cause

---

# 10. Root Cause

Candidate root causes are ranked using:

- dependency graph
- event chronology
- confidence
- communication failures
- protection relationships

---

# 11. Recommendations

Every active Incident may generate one or more Recommendations.

Examples:

- Inspect battery module 5.
- Verify RS485 connection.
- Restart adapter.
- Replace defective module.
- Contact manufacturer support.

Recommendations are advisory and explain their reasoning.

---

# 12. Automation

Automation is optional.

Before execution the engine evaluates:

- permissions
- interlocks
- safety rules
- approval requirements
- maintenance windows

Recommendation-first remains the default philosophy.

---

# 13. Integration with Health

Incidents influence:

- Device Health
- System Health
- Overall Health

Resolved incidents gradually lose influence according to configurable policies.

---

# 14. Audit

All acknowledgements, escalations, suppressions and automation actions are audited.

---

# 15. Acceptance Criteria

The model supports:

- immutable events
- alarm deduplication
- incident correlation
- explainable root cause
- recommendations
- safe automation
- replay of historical event streams
