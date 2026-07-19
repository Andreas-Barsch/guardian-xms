# 01 — Project Vision and Scope

**Project:** GUARDIAN xMS  
**Document type:** Architecture foundation  
**Status:** Draft  
**Version:** 0.1

---

## 1. Purpose

This document defines the vision, scope, boundaries and guiding principles of GUARDIAN xMS.

It establishes the architectural foundation for all subsequent domain models, technical decisions, implementation work and operational concepts.

---

## 2. Vision

GUARDIAN xMS is an open, modular and vendor-neutral management platform for technical systems.

The platform shall enable users to:

- inventory technical objects and their history,
- monitor devices, sensors and actuators,
- normalize data from heterogeneous sources,
- detect anomalies and failures,
- correlate alarms into incidents,
- calculate explainable health scores,
- analyse dependencies and impacts,
- generate recommendations,
- execute safe and controlled automations,
- support predictive maintenance and local AI-assisted analysis.

GUARDIAN xMS is designed as a general-purpose platform and is not limited to energy systems, Home Assistant or a specific manufacturer.

---

## 3. Origin

The first practical use case is the monitoring of a residential energy system with:

- Home Assistant Green,
- Hycube inverter and energy system,
- Pylontech battery modules,
- MQTT,
- external meters and sensors,
- future controllable loads.

This initial use case serves as a reference implementation and validation environment.

It must not constrain the core architecture to the energy domain.

---

## 4. Product Goals

GUARDIAN xMS shall provide:

1. A stable and historized CMDB for technical systems.
2. A vendor-neutral domain model.
3. A consistent identity model based on Guardian IDs.
4. A type and capability system for extensibility.
5. A relationship graph for topology, dependencies and impact analysis.
6. An event, alarm and incident lifecycle.
7. Explainable health scoring.
8. Recommendation-first automation.
9. Secure actuator control with explicit safeguards.
10. Support for local operation without mandatory cloud dependency.
11. Multi-site and multi-user capability.
12. A foundation for predictive analytics and AI assistance.

---

## 5. Primary Stakeholders

### 5.1 Operators

Users responsible for monitoring technical systems and reacting to incidents.

### 5.2 Administrators

Users responsible for configuration, permissions, integrations and lifecycle management.

### 5.3 Residents or End Users

Users who consume status information, recommendations and selected control functions.

### 5.4 Service Personnel

Temporarily authorized users who diagnose, maintain or replace devices.

### 5.5 Analysts

Users who evaluate historical data, trends, incidents and system performance.

### 5.6 Developers and Integrators

Contributors who extend GUARDIAN xMS with adapters, type packages, rules or user interfaces.

---

## 6. In Scope

The following capabilities are in scope:

### 6.1 Configuration Management

- Sites
- Areas
- Locations
- Systems
- Assets
- Devices
- Components
- Sensors
- Actuators
- type definitions
- capabilities
- external identities
- historized assignments
- historized relationships

### 6.2 Monitoring

- measurements
- states
- communication status
- data quality
- availability
- anomalies
- thresholds
- trends

### 6.3 Event and Incident Management

- event ingestion
- event normalization
- alarm creation
- deduplication
- correlation
- incident lifecycle
- acknowledgement
- escalation
- suppression
- root-cause candidates
- impact analysis
- closure and reopening

### 6.4 Health Management

- device health
- system health
- category health
- overall health
- confidence
- health contributions
- explainable score calculation

### 6.5 Automation and Recommendations

- recommendation generation
- rule evaluation
- approval workflows
- actuator commands
- interlocks
- safety checks
- fallback behaviour
- audit trail

### 6.6 Integration

- Home Assistant
- MQTT
- Modbus
- REST APIs
- device-specific protocols
- weather and forecast services
- notification services

### 6.7 Analytics and AI

- anomaly detection
- trend analysis
- outage runtime prediction
- predictive maintenance
- root-cause hypotheses
- natural-language explanations
- decision support

---

## 7. Out of Scope for the Initial Versions

The following are not initial goals:

- replacing Home Assistant as a complete smart-home platform,
- hard real-time industrial control,
- direct safety-critical control without independent safeguards,
- unrestricted autonomous AI control,
- manufacturer-specific logic in the core,
- mandatory cloud hosting,
- billing or ERP functionality,
- full enterprise IT service management,
- physical access control as a primary product function.

These areas may be integrated later through adapters or external systems.

---

## 8. Architectural Principles

### 8.1 Architecture Before Implementation

Core concepts and boundaries are documented before production code is introduced.

### 8.2 GitHub as the Single Source of Truth

All authoritative architecture, ADRs, source code, tests and documentation are maintained in the repository.

### 8.3 Domain-Driven Design

The software structure follows explicit domain concepts and bounded responsibilities.

### 8.4 Vendor-Neutral Core

Manufacturer-specific details belong in adapters, mappings or type packages.

### 8.5 Stable Identity

Every relevant domain object receives a stable Guardian ID.

### 8.6 Separation of Asset and Device

Functional roles and physical hardware have separate identities and lifecycles.

### 8.7 Relationships as First-Class Objects

Topology, dependencies, supply, communication, monitoring and control relationships are historized CMDB entities.

### 8.8 Configuration Before Code

New device families should be integrated through type definitions, capabilities, mappings and adapters wherever possible.

### 8.9 Explainability

Health scores, incidents, recommendations and automated decisions must be traceable to their inputs and rules.

### 8.10 Recommendation Before Automation

The system should initially recommend actions before executing them automatically.

### 8.11 Security by Design

Permissions, validity periods, auditability, interlocks and safe defaults are part of the architecture.

### 8.12 Local-First Operation

Core functionality should remain available locally and must not depend on an external cloud service.

### 8.13 Historization

Relevant states, assignments, relationships, permissions and decisions are preserved over time.

### 8.14 Testability

Architecture and implementation must support automated tests, replay and failure simulation.

---

## 9. Quality Goals

The most important quality attributes are:

1. **Reliability**  
   The system must continue to provide useful status information during partial failures.

2. **Maintainability**  
   Components, adapters and domain modules must remain understandable and replaceable.

3. **Extensibility**  
   New device types and integrations must not require changes to the core model.

4. **Explainability**  
   Users must understand why a health score, alarm or recommendation exists.

5. **Security**  
   Unauthorized or unsafe control actions must be prevented.

6. **Data Integrity**  
   Historical and current configuration data must remain consistent.

7. **Observability**  
   GUARDIAN xMS must monitor its own technical health.

8. **Portability**  
   The platform should support local, containerized and future distributed deployments.

9. **Usability**  
   Important states and incidents must be comprehensible without specialist knowledge.

10. **Performance**  
    The system must process continuous telemetry and events with predictable latency.

---

## 10. Initial Reference Scenario

The initial vertical slice shall cover:

```text
Pylontech data source
→ adapter
→ normalized measurements and states
→ CMDB object mapping
→ event detection
→ alarm
→ incident correlation
→ health score
→ dashboard
→ push notification
```

This scenario validates the entire architecture from data acquisition to user-visible outcome.

---

## 11. Success Criteria

The architecture is considered successful when:

- a new technical domain can be added without redesigning the core,
- a physical device can be replaced without losing the asset history,
- an incident can be traced to events, relationships and measurements,
- every health score can be explained,
- automated actions are auditable and protected,
- historical data can be replayed for testing,
- the first Pylontech use case works as a complete vertical slice,
- the platform can later support additional sites and system domains.

---

## 12. Repository References

This document is part of the architecture set.

Planned related documents:

- `02_Domain_and_CMDB_Model.md`
- `03_Logical_CMDB_Data_Model.md`
- `04_Type_and_Extension_Model.md`
- `05_Relationship_and_Dependency_Model.md`
- `06_Event_Alarm_and_Incident_Model.md`
- `07_Health_Model.md`
- `08_Automation_Model.md`
- `SAD.md`
- `ADR/`

---

## 13. Open Questions

The following decisions remain open:

- implementation language and framework,
- persistence technologies,
- event bus,
- API style,
- frontend technology,
- authentication and identity provider,
- deployment model,
- plugin isolation,
- licensing,
- release strategy.

These decisions will be documented through the SAD and dedicated ADRs.
