# 07 — Health Model

**Project:** GUARDIAN xMS
**Document type:** Runtime architecture
**Status:** Draft
**Version:** 0.7

---

# 1. Purpose

The Health Model defines how GUARDIAN xMS evaluates the operational condition of every managed object.

The model is:

- deterministic,
- explainable,
- vendor-neutral,
- configurable,
- hierarchical.

Every health value must be reproducible from its contributing factors.

---

# 2. Design Principles

Health is **not** a simple average.

It combines:

- current telemetry,
- active alarms,
- active incidents,
- historical reliability,
- communication quality,
- redundancy,
- confidence,
- missing information.

Every contribution is weighted.

---

# 3. Health Levels

```text
100–95   Excellent
94–85    Good
84–70    Degraded
69–50    Poor
49–20    Critical
19–0     Failed
```

---

# 4. Health Scope

Health is calculated for:

- Sensor
- Component
- Device
- Asset
- System
- Site
- Overall Installation

---

# 5. Contributors

Typical contributors:

- sensor plausibility
- threshold violations
- communication status
- active alarms
- active incidents
- redundancy
- maintenance state
- historical failures
- configuration validity
- battery balancing
- cell spread
- firmware compatibility

---

# 6. Generic Formula

```text
Health = 100
       - Σ(weight × penalty)
```

Penalty values are normalized to 0…1.

Weights are configurable per TypePackage.

---

# 7. Confidence

Each Health value carries a confidence score.

Influencing factors:

- telemetry age
- missing sensors
- communication loss
- inferred values
- data quality

Displayed separately from Health.

---

# 8. Hierarchical Aggregation

```text
Sensor
  ↓
Component
  ↓
Device
  ↓
Asset
  ↓
System
  ↓
Site
  ↓
Overall
```

Aggregation is weighted, not averaged equally.

Critical objects may dominate.

---

# 9. Historical Influence

Resolved incidents decay over time.

Policies may include:

- exponential decay
- linear decay
- fixed retention

Historic problems never disappear from the audit trail.

---

# 10. Explainability

Every Health calculation shall expose:

- contributing factors
- individual penalties
- weights
- final score
- confidence

Example:

```text
Battery Module 5

Health: 82 %

Contributors
-------------
Cell voltage spread        -8
Communication latency      -3
Incident history           -5
Firmware outdated          -2

Confidence: 98 %
```

---

# 11. Type-specific Extensions

TypePackages may define:

- additional contributors
- thresholds
- penalties
- aggregation rules

without changing the core algorithm.

---

# 12. Battery Example

Contributors:

- minimum cell voltage
- maximum cell voltage
- voltage spread
- temperature
- SOC consistency
- communication
- BMS alarms

These are combined according to configurable weights.

---

# 13. Health and Incidents

Health does not replace Incidents.

Incidents explain *why*.

Health summarizes *how well* the object currently operates.

---

# 14. Acceptance Criteria

The model provides:

- deterministic scores
- explainable calculations
- confidence values
- hierarchical aggregation
- configurable weights
- historical influence
- vendor neutrality
