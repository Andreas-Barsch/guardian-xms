# 09 — Adapter and Integration Architecture

**Project:** GUARDIAN xMS
**Document type:** Platform architecture
**Status:** Draft
**Version:** 0.9

---

# 1. Purpose

This document defines how GUARDIAN xMS integrates with external systems while keeping the Core completely independent of vendors and protocols.

Core principle:

> Core knows business semantics.
> Adapters know protocols.

---

# 2. Layered Architecture

```text
External Systems
        │
        ▼
+----------------------+
| Adapters             |
| MQTT REST Modbus ... |
+----------------------+
        │
        ▼
+----------------------+
| Mapping Layer        |
| Canonical Model      |
+----------------------+
        │
        ▼
+----------------------+
| Core Services        |
| Events Incidents     |
| Health Automation    |
+----------------------+
        │
        ▼
Persistence
```

---

# 3. Adapter Responsibilities

Adapters are responsible for:

- discovery
- authentication
- protocol handling
- retries
- reconnects
- parsing
- serialization
- command transport
- heartbeat
- connection quality

Adapters are **not** responsible for:

- CMDB logic
- health calculation
- incidents
- permissions
- automation decisions
- recommendation generation

---

# 4. Canonical Interface

Every adapter exposes the same logical operations:

```text
connect()
disconnect()
discover()
read()
subscribe()
write()
execute_command()
health()
capabilities()
```

The implementation is protocol-specific.

---

# 5. Supported Integration Types

Current targets include:

- MQTT
- REST
- WebSocket
- Modbus TCP
- Modbus RTU
- Serial
- CAN Gateway
- OPC UA
- SNMP

Future transports can be added without changing the Core.

---

# 6. Connector Packages

Each integration is packaged independently.

Examples:

```text
guardian.adapter.homeassistant
guardian.adapter.hycube
guardian.adapter.pylontech
guardian.adapter.sonoff
guardian.adapter.modbus
guardian.adapter.mqtt
```

Each adapter declares:

- version
- dependencies
- supported protocols
- supported TypePackages
- configuration schema

---

# 7. Discovery

Discovery workflow:

```text
Adapter
    ↓
External Identity
    ↓
Type Resolution
    ↓
Proposal
    ↓
Validation
    ↓
CMDB Update
```

Discovery may be manual, assisted or automatic.

---

# 8. Synchronisation

Adapters synchronize:

- telemetry
- topology
- firmware
- configuration
- availability
- alarms

Synchronization policies:

- polling
- subscription
- hybrid

---

# 9. Offline Behaviour

When communication fails:

- last values remain historized
- confidence decreases
- adapter health changes
- incidents may be created
- reconnect policy starts

No synthetic values overwrite real measurements.

---

# 10. Retry Strategy

Recommended strategy:

- exponential backoff
- jitter
- configurable limits
- circuit breaker
- heartbeat monitoring

---

# 11. Command Flow

```text
Recommendation
        ↓
Permission Check
        ↓
Safety Evaluation
        ↓
Command Definition
        ↓
Adapter
        ↓
Protocol
        ↓
Target Device
        ↓
Verification
```

Adapters return execution results only.

Business interpretation stays in the Core.

---

# 12. Home Assistant

Responsibilities:

- entity discovery
- service invocation
- event subscription
- state updates
- device registry mapping

---

# 13. Hycube

Responsibilities:

- inverter communication
- battery topology
- operating modes
- alarms
- command execution

---

# 14. Pylontech

Responsibilities:

- battery telemetry
- cell voltages
- SOC
- temperatures
- BMS alarms
- communication quality

---

# 15. Sonoff

Responsibilities:

- relay control
- state feedback
- availability
- power measurements (where supported)

---

# 16. Version Compatibility

Compatibility is evaluated between:

- Core
- Adapter
- TypePackage
- Device firmware
- External API

---

# 17. Security

Adapters must support:

- encrypted transport where available
- credential isolation
- least privilege
- audit logging
- secret rotation
- timeout handling

Secrets are never stored in source code.

---

# 18. Testing

Every adapter should provide:

- unit tests
- protocol mocks
- integration tests
- discovery fixtures
- command tests
- reconnect tests
- failure simulations

---

# 19. Acceptance Criteria

The architecture allows:

- new adapters without Core changes
- protocol-independent business logic
- deterministic mappings
- resilient reconnect behaviour
- isolated failures
- testable integrations
- vendor-neutral evolution

---

# 20. Related Documents

- 04 — Type and Extension Model
- 05 — Relationship Model
- 06 — Event, Alarm and Incident Model
- 08 — Recommendation and Automation Model
