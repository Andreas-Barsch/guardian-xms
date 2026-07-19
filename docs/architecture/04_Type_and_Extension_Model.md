# 04 — Type and Extension Model

**Project:** GUARDIAN xMS  
**Document type:** Type architecture  
**Status:** Draft  
**Version:** 0.4

---

## 1. Purpose

This document defines how GUARDIAN xMS describes technical object types, capabilities, metrics, states, commands, validation rules and vendor-specific extensions.

The model separates:

- runtime instances,
- reusable type definitions,
- vendor-neutral semantics,
- vendor-specific mappings,
- integration logic,
- migration logic.

The primary goal is to extend GUARDIAN xMS without changing the core domain model for every new device family.

---

## 2. Core Principle

> Instances represent real managed objects.  
> Types define what those objects are, what they can do and how they are validated.

Examples:

```text
Device instance:
DEV-000105

Device type:
Pylontech US2000C Battery Module
```

```text
Sensor instance:
SNS-000904

Sensor type:
Minimum Cell Voltage
```

```text
Actuator instance:
ACT-000087

Actuator type:
Battery Shutdown Command
```

---

## 3. Goals

The type system shall support:

1. vendor-neutral core behaviour,
2. reusable definitions,
3. explicit versioning,
4. capability-based composition,
5. validation,
6. UI metadata,
7. canonical metrics,
8. state and command definitions,
9. adapter mappings,
10. migrations,
11. package dependencies,
12. custom attributes,
13. backward compatibility.

---

## 4. Type Definition Hierarchy

Logical type entities include:

```text
SystemType
AssetType
DeviceType
ComponentType
SensorType
ActuatorType
RelationshipType
CapabilityType
MetricDefinition
StateDefinition
CommandDefinition
AttributeDefinition
ValidationRule
TypePackage
```

Each type definition is versioned independently or through its containing TypePackage.

---

## 5. TypePackage

A TypePackage groups related definitions for a domain, manufacturer, product family or integration.

Examples:

```text
guardian.core.energy
guardian.core.network
guardian.pylontech.us2000c
guardian.hycube.compact_neo
guardian.home_assistant
guardian.sonoff.minir4m
```

Logical structure:

```text
TypePackage
-----------
id
gid
package_key
display_name
description
version
status
publisher
license
repository_url
minimum_core_version
maximum_core_version
dependencies
created_at
published_at
deprecated_at
```

Rules:

- `package_key` is globally unique.
- versions follow semantic versioning.
- released versions are immutable.
- corrections require a new version.
- dependencies are explicit.
- package installation must be validated before activation.

---

## 6. Semantic Versioning

TypePackages use:

```text
MAJOR.MINOR.PATCH
```

Interpretation:

- `MAJOR`: incompatible model or mapping change,
- `MINOR`: backward-compatible capability or definition addition,
- `PATCH`: backward-compatible correction.

Examples:

```text
1.0.0
1.1.0
1.1.1
2.0.0
```

A package upgrade may require migrations.

---

## 7. SystemType

A SystemType defines a category of functional system.

Examples:

- battery storage system,
- photovoltaic system,
- heating system,
- network system,
- security system.

Logical structure:

```text
SystemType
----------
id
gid
type_package_id
type_key
display_name
description
version
status
allowed_child_system_types
allowed_asset_types
capabilities
attribute_definitions
validation_rules
ui_metadata
```

---

## 8. AssetType

An AssetType defines a persistent functional role.

Examples:

- battery module position,
- primary inverter,
- grid meter role,
- circulation pump role,
- rack switch role.

Logical structure:

```text
AssetType
---------
id
gid
type_package_id
type_key
display_name
description
version
status
allowed_device_types
minimum_assignments
maximum_assignments
capabilities
attribute_definitions
validation_rules
ui_metadata
```

Important rules:

- assignment cardinality is defined by AssetType,
- compatible DeviceTypes are explicit,
- the type may define required relationships,
- the type may define criticality defaults.

---

## 9. DeviceType

A DeviceType defines the characteristics of a physical device family.

Examples:

- Pylontech US2000C,
- Home Assistant Green,
- Sonoff MiniR4M,
- Modbus energy meter,
- managed Ethernet switch.

Logical structure:

```text
DeviceType
----------
id
gid
type_package_id
type_key
display_name
description
manufacturer
model_pattern
version
status
capabilities
component_templates
sensor_templates
actuator_templates
attribute_definitions
state_definitions
command_definitions
validation_rules
discovery_rules
mapping_definitions
ui_metadata
```

A DeviceType may instantiate default child objects.

Example:

```text
Pylontech US2000C
├── Component: Cell 1
├── Component: Cell 2
├── ...
├── Component: Cell 15
├── Component: BMS
├── Sensor: Module Voltage
├── Sensor: Current
├── Sensor: SOC
├── Sensor: Minimum Cell Voltage
├── Sensor: Maximum Cell Voltage
├── Sensor: Cell Spread
└── Sensor: Temperature
```

---

## 10. ComponentType

A ComponentType defines a subordinate part of a Device.

Examples:

- lithium cell,
- BMS board,
- relay channel,
- fan,
- communication interface,
- fuse.

Logical structure:

```text
ComponentType
-------------
id
gid
type_package_id
type_key
display_name
description
version
status
replaceable
serializable
allowed_child_component_types
capabilities
attribute_definitions
validation_rules
ui_metadata
```

---

## 11. SensorType

A SensorType defines the semantic meaning of a measurement or derived value.

Examples:

- cell voltage,
- module current,
- state of charge,
- temperature,
- communication state,
- calculated battery health.

Logical structure:

```text
SensorType
----------
id
gid
type_package_id
type_key
display_name
description
version
status
canonical_metric_id
native_unit
canonical_unit
value_type
minimum_value
maximum_value
precision
sampling_policy
quality_policy
attribute_definitions
validation_rules
ui_metadata
```

---

## 12. ActuatorType

An ActuatorType defines a controllable function.

Examples:

- relay output,
- battery shutdown,
- charge enable,
- inverter mode selection,
- load disconnect.

Logical structure:

```text
ActuatorType
------------
id
gid
type_package_id
type_key
display_name
description
version
status
safety_class
requires_approval
supported_commands
state_definitions
interlock_requirements
timeout_policy
fallback_policy
attribute_definitions
validation_rules
ui_metadata
```

---

## 13. Capability Model

Capabilities describe reusable behaviour independent of class inheritance.

Examples:

```text
measurable
switchable
controllable
firmware_updatable
battery_module
energy_storage
temperature_reporting
cell_voltage_reporting
supports_shutdown
supports_charge_control
supports_remote_reset
```

Logical structure:

```text
CapabilityType
--------------
id
gid
type_package_id
capability_key
display_name
description
version
status
required_metrics
required_states
supported_commands
required_attributes
validation_rules
```

A type may declare multiple capabilities.

Example:

```text
DeviceType: Pylontech US2000C

Capabilities:
- battery_module
- energy_storage
- cell_voltage_reporting
- temperature_reporting
- firmware_identifiable
```

---

## 14. Why Capabilities Instead of Deep Inheritance

Deep class inheritance creates rigid structures such as:

```text
Device
└── EnergyDevice
    └── BatteryDevice
        └── LithiumBattery
            └── PylontechBattery
```

This becomes difficult when a device belongs to multiple behavioural categories.

Capability composition is preferred:

```text
DeviceType
+ energy_storage
+ battery_module
+ temperature_reporting
+ communication_monitoring
```

This supports reuse and avoids manufacturer logic in the core.

---

## 15. MetricDefinition

MetricDefinition defines a canonical measured or calculated quantity.

Examples:

```text
voltage
current
power
energy
temperature
state_of_charge
cell_voltage_minimum
cell_voltage_maximum
cell_voltage_spread
communication_latency
health_score
```

Logical structure:

```text
MetricDefinition
----------------
id
gid
metric_key
display_name
description
value_type
canonical_unit
dimension
precision
aggregation_rules
quality_rules
status
```

Example:

```text
metric_key = battery.cell_voltage_spread
canonical_unit = V
value_type = decimal
```

---

## 16. Canonical Metrics

Adapters may receive vendor-specific fields.

Example:

```text
vendor field:
CellDiff

canonical metric:
battery.cell_voltage_spread
```

Canonical metrics allow:

- cross-vendor dashboards,
- generic health models,
- reusable incident rules,
- portable automation rules,
- consistent APIs.

---

## 17. StateDefinition

StateDefinition defines a valid state model.

Examples:

```text
online
offline
charging
discharging
idle
warning
alarm
maintenance
unknown
```

Logical structure:

```text
StateDefinition
---------------
id
gid
type_package_id
state_key
display_name
description
value_type
allowed_values
transition_rules
severity_mapping
terminal
status
```

State transitions may be validated.

Example:

```text
stored → installed → commissioning → active
```

Invalid:

```text
retired → active
```

unless an explicit reactivation workflow exists.

---

## 18. CommandDefinition

CommandDefinition defines an available action.

Examples:

```text
shutdown
restart
enable_charging
disable_charging
set_operating_mode
open_relay
close_relay
```

Logical structure:

```text
CommandDefinition
-----------------
id
gid
type_package_id
command_key
display_name
description
parameters
required_permissions
required_capabilities
safety_class
requires_approval
preconditions
interlocks
timeout
expected_state_change
fallback_command
status
```

Commands describe intent, not protocol details.

The adapter translates the canonical command into a device-specific operation.

---

## 19. AttributeDefinition

AttributeDefinition defines typed extensible fields.

Examples:

```text
nominal_capacity_kwh
cell_count
rated_voltage
maximum_charge_current
rack_position
communication_protocol
```

Logical structure:

```text
AttributeDefinition
-------------------
id
gid
type_package_id
attribute_key
display_name
description
applies_to
data_type
unit
required
default_value
minimum_value
maximum_value
allowed_values
validation_rule_id
sensitive
searchable
version
status
ui_metadata
```

---

## 20. ValidationRule

ValidationRule defines reusable constraints.

Examples:

- exactly 15 battery cells,
- nominal voltage within range,
- serial number format,
- one active Device assignment,
- valid relationship combinations,
- firmware compatibility,
- command preconditions.

Logical structure:

```text
ValidationRule
--------------
id
gid
type_package_id
rule_key
display_name
description
applies_to
expression_language
expression
severity
error_message
version
status
```

Validation rules must be deterministic and testable.

---

## 21. UI Metadata

Type definitions may provide presentation hints.

Examples:

```text
icon
category
display_order
unit_format
decimal_places
dashboard_group
default_chart
warning_threshold
critical_threshold
help_text
```

UI metadata must not contain business-critical logic.

The core remains authoritative for validation, safety and health calculation.

---

## 22. Templates

DeviceType may define templates for generated child instances.

### ComponentTemplate

```text
template_key
component_type_id
count
naming_pattern
position_pattern
required
```

### SensorTemplate

```text
template_key
sensor_type_id
owner_template
external_mapping
required
```

### ActuatorTemplate

```text
template_key
actuator_type_id
owner_template
required
```

Example:

```text
ComponentTemplate:
type = battery_cell
count = 15
naming_pattern = Cell {index}
position_pattern = {index}
```

---

## 23. Mapping Model

Mappings translate external data into canonical GUARDIAN xMS semantics.

Logical mapping types include:

```text
IdentityMapping
MetricMapping
StateMapping
CommandMapping
AttributeMapping
RelationshipMapping
```

Example:

```text
source:
Pylontech field "SOC"

target:
canonical metric "battery.state_of_charge"

conversion:
identity

unit:
percent
```

---

## 24. Adapter Model

Adapters are responsible for:

- communication,
- discovery,
- protocol handling,
- authentication,
- parsing,
- mapping,
- command transport,
- connection health,
- retries,
- rate limits.

Adapters are not responsible for:

- core CMDB identity,
- incident lifecycle,
- health score policy,
- authorization policy,
- global automation decisions.

Adapter output must use canonical models.

---

## 25. Discovery Workflow

A discovery workflow may follow:

```text
1. Adapter detects external object.
2. External identity is resolved.
3. Matching DeviceType is selected.
4. Proposed Device instance is created.
5. Templates create Components, Sensors and Actuators.
6. Relationships are proposed.
7. Validation rules run.
8. User or policy approves the result.
9. CMDB records become active.
```

Discovery may be:

```text
manual
assisted
automatic
```

Automatic activation requires a high-confidence policy.

---

## 26. Confidence and Provenance

Discovered definitions and mappings may include:

```text
confidence
provenance
discovered_at
verified_at
verified_by
```

Examples of provenance:

```text
manual
adapter
import
vendor_api
home_assistant
mqtt
inference
```

Low-confidence mappings must not silently trigger critical automation.

---

## 27. Type Migrations

Type definitions evolve.

A migration may:

- add an attribute,
- rename a metric,
- split a capability,
- introduce a component,
- change validation,
- deprecate a command,
- migrate external mappings.

Logical migration structure:

```text
TypeMigration
-------------
id
gid
type_package_id
from_version
to_version
migration_key
description
preconditions
operations
rollback_supported
status
created_at
```

Rules:

- migrations are explicit,
- migration execution is logged,
- failed migrations do not partially activate,
- historical data meaning must remain reconstructable,
- rollback strategy is documented.

---

## 28. Compatibility

Compatibility must be evaluated across:

- GUARDIAN xMS core version,
- TypePackage version,
- adapter version,
- Device firmware version,
- API version,
- stored instance schema.

A package may declare:

```text
minimum_core_version
maximum_core_version
compatible_adapter_versions
supported_firmware_ranges
```

---

## 29. Deprecation

A type definition may be:

```text
draft
active
deprecated
retired
```

Deprecated types:

- remain resolvable for historical data,
- may block new instance creation,
- may provide a recommended replacement,
- are not physically deleted.

Example:

```text
deprecated_type = guardian.pylontech.us2000c:1.0
replacement_type = guardian.pylontech.us2000c:2.0
```

---

## 30. Type Resolution

An instance must resolve to:

```text
type_key
type_version
type_package_version
```

Resolution must be deterministic.

Historical queries must use the type version applicable at the relevant time.

---

## 31. Example TypePackage: Pylontech US2000C

```text
package_key:
guardian.pylontech.us2000c

version:
1.0.0
```

Contents:

```text
DeviceType:
pylontech.us2000c

ComponentTypes:
battery_cell
battery_management_system

SensorTypes:
module_voltage
module_current
state_of_charge
minimum_cell_voltage
maximum_cell_voltage
cell_voltage_spread
temperature
communication_status

Capabilities:
battery_module
energy_storage
cell_voltage_reporting
temperature_reporting
communication_monitoring

MetricMappings:
Volt → battery.module_voltage
Curr → battery.module_current
SOC → battery.state_of_charge
CellMin → battery.cell_voltage_minimum
CellMax → battery.cell_voltage_maximum
CellDiff → battery.cell_voltage_spread
```

Component template:

```text
battery_cell count = 15
```

---

## 32. Example Command Mapping

Canonical command:

```text
shutdown
```

Adapter-specific implementation:

```text
protocol = Hycube API
endpoint = ...
method = ...
payload = ...
```

The canonical command definition contains:

- required permission,
- approval requirement,
- safety class,
- preconditions,
- expected state change.

The adapter contains only transport and protocol details.

---

## 33. Security Requirements

TypePackages may influence system behaviour and therefore require trust controls.

Requirements include:

- package origin,
- version integrity,
- optional signatures,
- dependency validation,
- restricted expression languages,
- no unrestricted executable code in declarative packages,
- migration review,
- permission review for commands,
- isolation of adapter code.

A TypePackage must not grant permissions by itself.

---

## 34. Testing Requirements

Each TypePackage should provide:

- schema tests,
- validation-rule tests,
- mapping tests,
- unit-conversion tests,
- discovery fixtures,
- command-mapping tests,
- migration tests,
- compatibility tests.

Example fixture:

```text
tests/fixtures/pylontech/us2000c/sample_console_output.txt
```

Expected normalized result:

```text
battery.module_voltage = ...
battery.state_of_charge = ...
battery.cell_voltage_spread = ...
```

---

## 35. Acceptance Criteria

The model is acceptable when:

1. a new DeviceType can be added without core schema changes,
2. vendor fields can map to canonical metrics,
3. a DeviceType can compose multiple capabilities,
4. child Components and Sensors can be generated from templates,
5. types can evolve through explicit migrations,
6. historical data remains interpretable,
7. commands remain independent of transport protocols,
8. package compatibility is verifiable,
9. validation rules are testable,
10. unsafe package behaviour is constrained.

---

## 36. Consequences for Implementation

The implementation requires:

- a TypePackage registry,
- package validation,
- type resolution,
- semantic version support,
- dependency resolution,
- instance-to-type references,
- capability lookup,
- template expansion,
- mapping execution,
- migration execution,
- package activation states,
- package audit records,
- package tests.

---

## 37. Related Architecture Decisions

This document is supported by:

- ADR-003 — Versioned Type Definitions and TypePackages

Related decisions:

- ADR-001 — Separate Asset and Device
- ADR-002 — Stable Guardian ID
- ADR-004 — Relationships as First-Class CMDB Objects

---

## 38. Open Decisions

- declarative format for TypePackages,
- package directory structure,
- package signing model,
- package registry design,
- expression language,
- migration language,
- plugin sandboxing,
- adapter packaging,
- UI metadata schema,
- package review and publication workflow.
