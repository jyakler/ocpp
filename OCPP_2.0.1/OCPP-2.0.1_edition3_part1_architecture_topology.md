# OCPP 2.0.

## Part 1 - Architecture & Topology

## Edition 3 FINAL, 2024-05-


## Table of Contents

- Disclaimer
- Version History
- 1. Introduction.
   - 1.1. Goal of this document
   - 1.2. Terms and abbreviations
- 2. 3-tier model
- 3. Information Model
- 4. Device Model: Addressing Components and Variables.
   - 4.1. Components
   - 4.2. Variables
   - 4.3. Characteristics and Attributes
   - 4.4. Monitoring
   - 4.5. Standardized lists of Components and Variables
   - 4.6. Minimum Device Model
- 5. Information Model vs. Device Model
- 6. Using OCPP for other purposes than EV charging
- 7. Numbering
   - 7.1. EVSE numbering
   - 7.2. Connector numbering
   - 7.3. Transaction IDs.
- 8. Topologies supported by OCPP
   - 8.1. Charging Station(s) directly connected to CSMS
   - 8.2. Multiple Charging Stations connected to CSMS via Local Proxy
   - 8.3. Multiple Charging Stations connected to CSMS via Local Controller
   - 8.4. Non-OCPP Charging Stations connected to CSMS via OCPP Local Controller.
   - 8.5. DSO control signals to CSMS
   - 8.6. Parallel control by CSMS and EMS.
- 9. Part 1 Appendix: OCPP Information Model.
   - 9.1. Explanation of UML representation and message generation
   - 9.2. Visual Representation of OCPP Information Model


## Disclaimer

Copyright © 2010 – 2024 Open Charge Alliance. All rights reserved.

This document is made available under the _*Creative Commons Attribution-NoDerivatives 4.0 International Public License*_

(https://creativecommons.org/licenses/by-nd/4.0/legalcode).


## Version History

**Version Date Description**

2.0.1 Edition 3

2024-05-06 OCPP 2.0.1 Edition 3. All errata from OCPP 2.0.1 Part 1 until and including Errata

2024-04 have been merged into this version of the specification.

2.0.

2020-03-31 Final version of OCPP 2.0.

2.

2018-04-

OCPP 2.0 April 2018

First release of this Architecture & Topology document


## 1. Introduction.

### 1.1. Goal of this document

The goal of this document is to describe a number of architecture related topics for OCPP 2.0.1.

OCPP was originally intended for two way communication between a backoffice, in OCPP the _Charging Station Management System_

(in this document: CSMS) and a Charging Station. The protocol has become more advanced and with every new revision new

functionalities and options are added. It has evolved into a protocol that can be used in different architectures for different types of

Charging Stations.

This document describes, in addition to the original "simple" setup CSMS <> Charging Station, a number of topologies as an

additional explanation for using OCPP. Furthermore, the Device Management concept to configure and monitor any type of

Charging Station, the OCPP Information Model and the 3-tier model are explained.

This document is partially **informative** and partially **normative** and is not intended to limit the use of OCPP. However, it does add an

explanation what kind of use of OCPP the creators of OCPP had in mind when creating this version of the specification. This

document is therefore also intended to support the reader of the protocol specification in Part 2 of OCPP to understand how it can

be used.

### 1.2. Terms and abbreviations

This section contains the terminology and abbreviations that are used throughout this document.

**1.2.1. Terms**

**Term Meaning**

**Charging Station** The Charging Station is the physical system where EVs can be charged. A Charging Station

has one or more EVSEs.

**Connector** The term Connector, as used in this specification, refers to an independently operated and

managed electrical outlet on a Charging Station. In other words, this corresponds to a single

physical Connector. In some cases an EVSE may have multiple physical socket types and/or

tethered cable/Connector arrangements(i.e. Connectors) to facilitate different vehicle types

(e.g. four-wheeled EVs and electric scooters).

**EVSE** An EVSE is considered as an independently operated and managed part of the Charging

Station that can deliver energy to one EV at a time.

**Local port Smart Meter** The Local port on a Smart Meter is a port (for example serial) on a digital electricity meter

that provides access to information about meter readings and usage.

**1.2.2. Abbreviations**

**Abbreviation Meaning**

DSO Distribution System Operator

CSO Charging Station Operator

CSMS Charging Station Management System

EMS Energy Management System. In this document this is defined as a device that manages the local loads

(consumption an production) based on local and/or contractual constraints and/or contractual incentives. It

has additional inputs, such as sensors and controls from e.g. PV, battery storage.

EVSE Electric Vehicle Supply Equipment

LC Local Controller. In this document this is defined as a device that can send messages to its Charging

Stations, independently of the CSMS. A typical usage for this is the local smart charging case described in

the Smart Charging chapter of Part 2 of OCPP, where a Local Controller can impose charge limits on its

Charging Stations.

LP Local Proxy. Acts as a message router.


## 2. 3-tier model

_This section is informative._

To understand the terminology in the OCPP specification, it is important to understand the starting point of this specification. The

OCPP specification uses the term Charging Station as the physical system where EVs can be charged. A Charging Station can have

one or more EVSEs (Electric Vehicle Supply Equipment). An EVSE is considered as a part of the Charging Station that can deliver

energy to one EV at a time. The term Connector, as used in this specification, refers to an independently operated and managed

electrical outlet on a Charging Station, in other words, this corresponds to a single physical Connector. In some cases an EVSE may

have multiple physical socket types and/or tethered cable/connector arrangements to facilitate different vehicle types (e.g. four-

wheeled EVs and electric scooters). This setup is referred to as the 3-tier model and visualized in the figure below.

_Figure 1. 3-tier model as used in OCPP_

**NOTE**

This section describes the charging infrastructure on a logical level for communication purposes. We do not wish

to impose a mapping onto physical hardware. This is a manufacturer’s choice. For example, the EVSE might be

integrated into a Charging Station and to look as just a part of that device, but it might just as well have its own

casing and live outside of the physical entity Charging Station, for example a charging plaza with 20 EVSEs and

Connectors which communicates via 1 modem as 1 Charging Station to the CSMS is seen by OCPP as 1 Charging

Station.


## 3. Information Model

_This section is informative._

Given the growing complexity of the messages of OCPP, OCPP 2.0.1 is based on an _Information Model_ as a blueprint for the

messages and inherent schemas of OCPP. With an information model, we mean a logical object set, describing real objects with all

their properties. This provides an informative representation of information structure in the protocol. Furthermore, it enables

making objects within OCPP reusable and enables consistent definition of messages and automatically generated message

schemas (Part 3).

The Information Model is a model, also called Domain Model or Core Model, based on which the OCPP messages and datatypes

are generated. These datatypes are extracted from the the OCPP 1.6 specification and are named Core DataTypes and Qualified

DataTypes. The figure below illustrates how the DataTypes in the information model are built up.

In part 2 - Specification, chapter Datatypes, some DataTypes have the Common: prefix. This originates from the Information Model.

It means that the DataType is able to be shared among other DataTypes and Messages. This has no impact on the OCPP

implementation of a device.

```
«BDTSimpleType»
```
```
IntegerType
```
```
«Facet»
```
```
+ fractionDigits = 0 {readOnly}
```
```
(from QualifiedDataTypes)
```
```
«CDTSimpleType,LDT,EC...
```
```
NumericType
```
```
«Supplementary»
```
```
+ format: String [0..1]
```
```
(from CoreDataTypes)
```
```
«PRIMSimpleType»
```
```
Decimal
```
```
(from PrimitiveDataTypes)
```
```
anySimpleType
```
```
«XSDsimpleType»
```
```
XSDDatatypes::decimal
```
```
«BDTSimpleType»
```
```
PercentageType
```
```
«Facet»
```
```
+ fractionDigits = 0 {readOnly}
```
```
+ minInclusive = 0 {readOnly}
```
```
+ maxInclusive = 100 {readOnly}
```
```
(from QualifiedDataTypes)
```
```
«BDTComplexType»
```
```
PositiveAmount
```
```
«Facet»
```
```
+ fractionDigits = 0 {readOnly}
```
```
+ minInclusive = 0 {readOnly}
```
```
(from QualifiedDataTypes)
```
```
«CDTComplexType,ECDMChange,...
```
```
AmountType
```
```
«Supplementary»
```
```
+ currency: String [0..1]
```
```
+ currencyCodeListVersion: String [0..1]
```
```
(from CoreDataTypes)
```
```
«BDTComplexType»
```
```
PowerType
```
```
(from QualifiedDataTypes)
```
```
«CDTComplexType,LDT,ECDMChange,...
```
```
MeasureType
```
```
«Supplementary»
```
```
+ measureUnit: String [0..1]
```
```
+ measureUnit.CodeListVersion: String [0..1]
```
```
(from CoreDataTypes)
```
```
«CDTComplexType,LDT,ECDMChange,CDT»
```
```
IdentifierType
```
```
«Supplementary»
```
```
+ identificationScheme: String [ 0 .. 1 ]
```
```
+ identificationScheme.Agency: String [ 0 .. 1 ]
```
```
+ identificationScheme.AgencyName: String [ 0 .. 1 ]
```
```
+ identificationScheme.Name: String [ 0 .. 1 ]
```
```
+ identificationScheme.UniformResource: String [ 0 .. 1 ]
```
```
+ identificationScheme.Version: String [ 0 .. 1 ]
```
```
+ identificationSchemeData.UniformResource: String [ 0 .. 1 ]
```
```
(from CoreDataTypes)
```
```
«PRIMSimpleType»
```
```
NormalizedString
```
```
(from PrimitiveDataTypes)
```
```
string
```
```
«XSDsimpleType»
```
```
XSDDatatypes::normalizedString
```
```
«BDTComplexType»
```
```
CertificateSerialNumberType
```
```
«Facet»
```
```
+ maxLength = 20 {readOnly}
```
```
(from QualifiedDataTypes)
```
_Figure 2. Example datatypes_

The Information Model is divided into a number of "functions" to have a better overview of the model (thus for readability):

- Transactions
- SmartCharging
- Metering
- Security (Profiles/Authorization)
- Communication
- SecondaryActorSchedule

For more details about the actual model per function, please refer to the appendix.


## 4. Device Model: Addressing Components and Variables.

The Device Model refers to a generalized mechanism within OCPP to enable any model of Charging Station to report how it is build

up, so it can be managed from any CSMS. To manage a Charging Station with the Device Model (i.e. "to manage a device") a

number of messages and use cases is defined to configure and monitor a Charging Station in detail, without defining the structure

of the Charging Station in advance. To be able do do this, OCPP provides a generalized mechanism to allow the exchange of a wide

range of information about Charging Station. This version of the Device Model has the 3-tier model (Charging Station, EVSE,

Connector) as its starting point, which means that any description created with the Device Model follows these three tiers. The

remainder of this chapter describes how the data (and associated meta-data) looks like that can be exchanged between a Charging

Station and a CSMS. The use cases and messages that are used to manage a device are _not_ described here, but in Part 2 of the

specification. This chapter only focuses on the data model.

### 4.1. Components

In OCPP 2.0.1, a Charging Station is modelled as a set of _"Components"_ , typically representing physical devices (including any

external equipment to which it is connected for data gathering and/or control), logical functionality, or logical data entities.

_Components_ of different types are primarily identified by a ComponentName, that is either the name of a _standardized_ component

(see OCPP part 2c), or a custom/non-standardized component name, for new, pre-standardized equipment, vendor specific

extensions, etc.

_ChargingStation_ (TopLevel), _EVSE_ , and _Connector_ represent the three major "tiers" of a Charging Station, and constitute an implicit

"location-based" addressing scheme that is widely used in many OCPP data structures. Each "tier" has a component of the same

name, which represents the tier. For example, EVSE 1 on a Charging Station is represented by the component named "EVSE" (no

instance name) with " _evseId_ = 1". In the same manner, Connector 1 on EVSE 1 is represented by the component named "Connector"

(no instance name) with " _evseId_ = 1, _connectorId_ = 1".

By default, all _components_ are located at the _ChargingStation_ tier, but individual instances of any component can be associated with

a specific _EVSE_ , or a specific _Connector_ (on a specific EVSE) by including EVSE or EVSE and Connector identification numbers as

part of a component addressing reference.

Additionally, there can be more than one instance of a component (in the functional dimension), representing multi-occurrence

physical or logical components (e.g. power converter modules, fan banks, resident firmware images, etc.).

Each distinct _component_ instance is uniquely identified by an (optional) _componentInstance_ addressing key. When no

_componentInstance_ is provided, then the default or only instance of a _component_ is referenced.

_Components_ do not in themselves hold data: all externally accessible data associated with each component instance is

represented by a set of _variables_ that can be read, set, and/or monitored for changes. The relationship of a Component with one or

more Variables is illustrated in below.

«BusinessComponent»

**VariableType**

+ Instance: NameIdentifierType [ 0 .. 1 ]

+ Name: NameIdentifierType

«BusinessComponent»

**ComponentType**

+ Instance: NameIdentifierType [ 0 .. 1 ]

+ Name: NameIdentifierType

«BusinessComponent»

**EVSEType**

+ ConnectorId: NumericIdentifierType [ 0 .. 1 ]

«Content»

+ MRID: NumericIdentifierType

*

1

0 1

_Figure 3. Component and variables_

The table below illustrates some common components (by their standardized component-names), and examples of the hierarchical

location levels at which they typically occur for a basic home charger and a typical public Charging Station.

**Basic home charger example configuration**

**ChargingStation tier EVSE tier Connector tier**

ChargingStation (itself, as a whole) EVSE (itself, as a whole) Connector (itself, as a whole)

RadioLink ControlMetering PlugRetentionLock

TokenReader OverCurrentBreaker

Controller RCD


**Basic home charger example configuration**

ChargingStatusIndicator

**Public Charging Station example configuration**

**ChargingStation tier EVSE tier Connector tier**

ChargingStation (itself, as a whole) EVSE (itself, as a whole) Connector (itself, as a whole)

ElectricalFeed ElectricalFeed AccessProtection

TokenReader TokenReader PlugRetentionLock

Display Display

FiscalMetering FiscalMetering

Clock ControlMetering

Controller OverCurrentBreaker

RCD

ChargingStatusIndicator

### 4.2. Variables

Every _component_ has a number of _variables_ , that can, as appropriate, be used to hold, set, read, and/or report on all (externally

visible) data applicable to that _component_ , including configuration parameters, measured values (e.g. a current or a temperature)

and/or monitored changes to variable values.

Although many _components_ can have associated _variables_ that are, by their nature, specific to the component type (e.g.

_ConnectorType_ for a _Connector_ component), there are a minimal set of standardized _variables_ that are used to provide standardized

high level event notification and state/status reporting (e.g. _Problem_ , _Active_ ) on a global and/or selective basis, and also to report

component presence, availability, etc. during the inventorying/discovery process (e.g. _Available_ , _Enabled_ ). A Charging Station is not

required to report the base variables: _Present_ , _Available_ and _Enabled_ when they are readonly and set to _true_. When a Charging

Station does not report: _Present_ , _Available_ and/or _Enabled_ the Central System SHALL assume them to be readonly and set to _true_

Variables can be any of a range of common general-purpose data types (boolean, integer, decimal, date-time, string), but also can

have their allowable values constrained to particular ranges, enumeration lists, sets, or ordered lists.

To support complex components, there can be more than one instance of any given variable name associated with any

components (e.g. power converter modules reporting temperature, current, or voltage at multiple points).

Each distinct _variable_ instance is uniquely identified by an (optional) _variableInstance_ addressing key string value. When no

_variableInstance_ is provided, then the default or only instance of a _variable_ is referenced.

### 4.3. Characteristics and Attributes

Each _variable_ , in addition to its primary ( _"Actual"_ ) value, can have a set of associated secondary data that is linked to the same

primary _variable_ name and _variableInstance_.

This greatly avoids cluttering the _variables_ namespace with confusing clusters of ancillary variable names (e.g. FanSpeed,

FanSpeedUnits, MinimumFanSpeed, BaseFanSpeed) that lack consistence and discoverability.

The ancillary variable data includes:

- Variable characteristics meta-data (read-only)

◦

Unit of measure (V,W,kW,kWh, etc.)

◦

Data type (Integer, Decimal, String, Date, OptionList, etc.)

◦

Lower limit

◦

Upper limit

◦

List of allowed values for enumerated variables

- Variable attributes (read-write):

◦

Actual value

◦

Target value

◦

Configured lower limit

◦

Configured upper limit

◦

Mutability (whether the value can be altered or not, e.g. ReadOnly or ReadWrite)

◦

Persistence (whether the value is preserved in case of a reboot or power loss)


The relationship of a Variable with one or more VariableAttributes is illustrated in the figure below.

«BusinessComponent»

**VariableType**

+ Instance: NameIdentifierType [ 0 .. 1 ]

+ Name: NameIdentifierType

«BusinessComponent»

**VariableAttributeType**

+ Constant: IndicatorType

+ Mutability: MutabilityEnumType [0..1]

+ Persistent: IndicatorType

+ Type: AttributeEnumType [ 0 .. 1 ]

+ Value: Base64String2500Type

«BusinessComponent»

**VariableCharacteristicsType**

+ DataType: DataEnumType

+ MaxLimit: AmountType [0..1]

+ MinLimit: AmountType [0..1]

+ SupportsMonitoring: IndicatorType

+ Unit: MeasurementUnitType [0..1]

+ ValuesList: CI1000TextType [0..1]

«BusinessComponent»

**ComponentType**

+ Instance: NameIdentifierType [ 0 .. 1 ]

+ Name: NameIdentifierType

«BusinessComponent»

**EVSEType**

+ ConnectorId: NumericIdentifierType [ 0 .. 1 ]

«Content»

+ MRID: NumericIdentifierType

1

0..

*

1

0

1

**1**

1..*

_Figure 4. Variable attributes and characteristics_

There is a difference between how to implement (physical) devices and (virtual) controller components, using the DeviceModel. A

(virtual) controller component has to be implementing as described in part 2 chapter the "Referenced Components and Variables".

These kind of components/variables are only using the variableAttribute type 'Actual'. Depending on if this variableAttribute is

writable, the CSMS can use this to set a new value.

(Physical) devices are a bit more complex to implement. For example, there is a fan with a fan speed, that has a (physical) limit with

a range of 0 - 1000. But it should not be allowed to set the value below 200, because the fan can stop functioning. And it should not

be set above 500, because that would be bad for the fan on the long run. When implementing this device using the DeviceModel, it

can be defined as follows:

**Component name** Fan

**Variable name** FanSpeed

**variableAttribute 1 type** Actual

**value** <The current fan speed value of the fan.>

**mutability** ReadOnly

**variableAttribute 2 type** Target

**value** <The CSMS can use this value to adjust the fan

speed. The Charging Station SHALL try to keep the

actual value at the target value.>

**mutability** ReadWrite

**variableAttribute 3 type** MaxSet

**value** <The value '500' from the example. The target may

not be set above this value.>

**variableAttribute 4 type** MinSet

**value** <The value '200' from the example. The target may

not be set below this value.>

**variableCharacteristics maxLimit** <The value '1000' from the example. This could be the

physical max limit of the fan.>

**minLimit** <The value '0' from the example. This could be the

physical min limit of the fan. This could also be -1000,

if the fan is also able to rotate in the other direction.>

**Description** This is an example of how a fan could be defined using the DeviceModel.

When trying to set the target with value 600, the Charging Station will first check the allowed min and max values/limits and reject


the set. If the target value is set to 500, the value is within range and the Charging Station will allow the set and start to adjust the

actual fan speed. If the actual fan speed is measured to be 502, it’s out of range. But it should be reported to the CSMS, so the

actual value of a physical component should be updated without checking the min and max values/limits.

### 4.4. Monitoring

Optional monitoring settings can be associated with a variable, that allow changes to _variable_ ( _Actual_ ) values are to be reported to

the CSMS as event notifications.

These include:

- Monitoring value
- Monitoring type: upper threshold, lower threshold, delta, periodic
- Severity level when reporting the event

The following table show which MonitorType/dataType combinations are possible.

**string decimal integer dateTime boolean OptionList SequenceList MemberList**

**UpperThresh**

**old**

X X

**LowerThresh**

**old**

X X

**Delta** X X X X X X X X

**Periodic** X X X X X X X

**PeriodicCloc**

**kAligned**

X X X X X X X

- For _UpperThreshold_ and _LowerThreshold_ the value represents the to be exceeded value by the actual value of the variable.
- For _Delta_ this value represents the change in value comparing with the actual value from the moment the monitor was set.

◦

When the dataType of the variable is integer or decimal, this value represents the difference to be reached to trigger

the monitor.

◦

When the dataType of the variable is dateTime the unit of measure will be in seconds.

◦

When the dataType of the variable is string, boolean, OptionList, SequenceList or MemberList, this value is ignored.

The monitor will be triggered by every change in the actual value.

- When a delta monitor is triggered OR when the Charging Station has rebooted, the Charging Station shall set a new

momentary value.

- For _Periodic_ and _PeriodicClockAligned_ the value represents the interval in seconds.

The relationship between a Variable and one or more VariableMonitoring elements is illustrated in the figure below.


```
«BusinessComponent»
```
```
VariableType
```
```
+ Instance: NameIdentifierType [ 0 .. 1 ]
```
```
+ Name: NameIdentifierType
```
```
«BusinessComponent»
```
```
VariableAttributeType
```
```
+ Constant: IndicatorType
```
```
+ Mutability: MutabilityEnumType [0..1]
```
```
+ Persistent: IndicatorType
```
```
+ Type: AttributeEnumType [ 0 .. 1 ]
```
```
+ Value: Base64String2500Type
```
```
«BusinessComponent»
```
```
VariableCharacteristicsType
```
```
+ DataType: DataEnumType
```
```
+ MaxLimit: AmountType [0..1]
```
```
+ MinLimit: AmountType [0..1]
```
```
+ SupportsMonitoring: IndicatorType
```
```
+ Unit: MeasurementUnitType [0..1]
```
```
+ ValuesList: CI1000TextType [0..1]
```
```
«BusinessComponent»
```
```
ComponentType
```
```
+ Instance: NameIdentifierType [ 0 .. 1 ]
```
```
+ Name: NameIdentifierType
```
```
«BusinessComponent»
```
```
VariableMonitoringType
```
```
+ Id: NumericIdentifierType
```
```
+ Severity: NumericIdentifierType
```
```
+ Transaction: IndicatorType
```
```
+ Type: MonitorEnumType
```
```
+ Value: MeasureType
```
```
«BusinessComponent»
```
```
EVSEType
```
```
+ ConnectorId: NumericIdentifierType [ 0 .. 1 ]
```
```
«Content»
```
```
+ MRID: NumericIdentifierType
```
```
1
```
```
0..
```
```
*
```
```
1
```
```
1
```
```
1..*
```
```
*
```
```
1
```
```
0 1
```
_Figure 5. Variables and monitoring_

### 4.5. Standardized lists of Components and Variables

To provide some level of interoperability between different Charging Stations and CSMSs, besides the above defined model of

_Components_ and _Variables_ , part 2 - appendices of the OCPP specification provides a list of standardized names for Components

and Variables. The idea of this lists is to make sure that _if_ a Charging Station and CSMS want to exchange information about a

component, they both use the same name and description _if_ it is listed in the OCPP specification. For names of a _Components_ or

_Variables_ that are not listed in the specification, bilateral appointments between Charging Station manufacturer and CSMS are to be

made. In these cases it is advised to provide feedback to the Open Charge Alliance to be able to include new/additional

_Components_ and _Variables_ in new versions of OCPP.

### 4.6. Minimum Device Model

Since the Device Model is a _generalized_ mechanism which can be applied to any model of Charging Station, the complexity of

different implementations can vary. It consists of a number of use cases and messages that are not all required. This section

describes the minimum part of the Device Model that needs to be implemented to create a working implementation of OCPP 2.0.1.

The Device Model introduces Components and Variables that can be used for configuring and monitoring a Charging Station. A

number of these Components and Variables are included in the list of _Referenced Components and Variables_ (grouped by

Functional Block) in Part 2 of the specification. When implementing a Functional Block, ALL required Configuration Variables that

belong to a Functional Block SHALL be implemented. The required Configuration Variables from the _General_ section SHALL also be

implemented for all implementations of OCPP 2.0.1.

The following table describes which messages are required or optional to implement for all use cases that are part of the Device

Model implementation.

**Use cases / messages that are part of a minimium Device Model implementation**

**Use case Messages**

_B05 Set Variables_ SetVariables message MUST be implemented

_B06 Get Variables_ GetVariables message MUST be implemented.

_B07 Get Base Report_ GetBaseReport message MUST be implemented and MUST support ConfigurationInventory

and FullInventory. The content of these reports depends on the implementation of the

Charging Station. It is up to the implementer to decide which components and variables exist

in the implementation.

**Additional use cases / messages that are** **_not_** **part of a minimium Device Model implementation**

**Use case Messages**

_B08 Get Custom Report_ GetCustomReport message is optional.

_N02 Get Monitoring Report_ GetMonitoringReportRequest message is optional.

_N03 Set Monitoring Base_ SetMonitoringBaseRequest message is optional.

_N04 Set Variable Monitoring_ SetVariableMonitoringRequest message is optional.


**Use cases / messages that are part of a minimium Device Model implementation**

_N05 Set Monitoring Level_ SetMonitoringLevelRequest message is optional.

_N06 Clear/Remove Monitoring_ ClearVariableMonitoringRequest message is optional.

_N07 Alert Event_ it is RECOMMENDED that NotifyEventRequest is implemented in the Charging Station even

when monitoring is not implemented, so that this can be used to report built-in monitoring

events.

_N08 Periodic Event_ see N07.


## 5. Information Model vs. Device Model

As described above, the terms Information Model and Device Model refer to different concepts. The Information Model refers to a

model of the information structure upon which the messages and datatypes in OCPP are based, whereas the Device Model refers

to a generalized mechanism within OCPP to enable any model of Charging Station to report how it is build up so, it can be managed

from any CSMS without defining the structure of the Charging Station in advance.

The messages that are used for Device Management are therefore part of the Information Model and the objects that are used for

modelling a device ( _'Component'_ and _'Variable'_ ) are also part of the Information Model.


## 6. Using OCPP for other purposes than EV charging

As indicated in the introduction of this document, OCPP is primarily intended for two way communication between a CSMS and a

Charging Station. However, with the addition of the Device Model as described in the chapter Device Model, OCPP can additionally

be used for other purposes. For example, the reporting of Events or Status changes in transformers or stand-alone battery packs

might also be useful for companies that are rolling out EV charging infrastructure. In this example, a BootNotification could be used

to connect these devices to a management system. In the device model a device that is not a Charging Station, can be recognized

by the fact that the component Charging Station is not present at the top level. At the moment the OCPP specification does not

provide use cases for non Charging Station devices. However, they may be added in a future version of OCPP.


## 7. Numbering

_This section is normative._

### 7.1. EVSE numbering

To enable the CSMS to address all the EVSEs of a Charging Station, EVSEs MUST always be numbered in the same way.

EVSEs numbering (evseIds) MUST be as follows:

- The EVSEs MUST be sequentially numbered, starting from 1 at every Charging Station (no numbers may be skipped).
- evseIds MUST never be higher than the total number of EVSEs of a Charging Station
- For operations initiated by the CSMS, evseId 0 is reserved for addressing the entire Charging Station.
- For operations initiated by the Charging Station (when reporting), evseId 0 is reserved for the Charging Station main

controller.

Example: A Charging Station with 3 EVSEs: All EVSEs MUST be numbered with the IDs: 1, 2 and 3. It is advisable to number the

EVSEs of a Charging Station in a logical way: from left to right, top to bottom incrementing.

### 7.2. Connector numbering

To enable the CSMS to address all the Connectors of a Charging Station, Connectors MUST always be numbered in the same way.

Connector numbering (connectorIds) MUST be as follows:

- The connectors are numbered (increasing) starting at connectorId 1 on every EVSE
- Every connector per EVSE has a unique number
- ID of the first Connector of an EVSE MUST be 1
- Additional Connectors of the same EVSE MUST be sequentially numbered (no numbers may be skipped)
- connectorIds MUST never be higher than the total number of connectors on that EVSE

Example: A Charging Station with 3 EVSEs that each have 2 connectors, is numbered as follows:

- EVSE 1 has connectors with connectorId 1 and 2
- EVSE 2 has connectors with connectorId 1 and 2
- EVSE 3 has connectors with connectorId 1 and 2

### 7.3. Transaction IDs.

TransactionIds are now generated by the Charging Station and MUST be unique on this Charging Station for every started

transaction.

In OCPP 1.x this was done by the CSMS.

The format of the transaction ID is left to implementation. This MAY for example be an incremental number or an UUID.


## 8. Topologies supported by OCPP

This chapter shows a number of topologies for using OCPP. As indicated in the introduction, OCPP was originally used for a setup

where each Charging Station communicates directly with the CSMS. It is important to keep in mind that OCPP has no knowledge of

the topology of the Charging Station network. The following figure shows the possible components in a setup using OCPP and the

relations between these components:

_Figure 6. Possible components in a setup using OCPP_

### 8.1. Charging Station(s) directly connected to CSMS

**Description**

This is the basic setup for using OCPP.

CSMS

Charging

Station

```
OCPP
```
_Figure 7. Charging Station directly connected to CSMS_

### 8.2. Multiple Charging Stations connected to CSMS via Local Proxy

**Description**

In some situations it is desirable to route all communications for a group of Charging Stations through a single network node (i.e.

modem, router, etc.). A typical example is the situation where a number of a Charging Stations are located in an underground

parking garage with little or no access to the mobile network. In order to provide access to mobile data the Charging Stations are

linked to a central data communications unit over a LAN. This central unit connects to the mobile network and acts as a proxy

between CSMS and Charging Stations. Such a unit is called a "local proxy" (LP) in OCPP. A local proxy acts as a message router.

Neither the CSMS nor the Charging Stations are aware of the topology of the network. For the Charging Stations in the group the

local proxy "is" the CSMS. Similarly, for the CSMS the local proxy "is" the Charging Station. The diagram below illustrates this

configuration.

CSMS LP

Charging

Station 1

Charging

Station n

```
OCPP
```
```
OCPP
```
```
OCPP
```
_Figure 8. Multiple Charging Stations connected to CSMS via Local Proxy_


### 8.3. Multiple Charging Stations connected to CSMS via Local Controller

**Description**

Whereas a local proxy does little more than route OCPP messages, a Local Controller can send messages to its Charging Stations,

independently of the CSMS. A typical usage for this is the local smart charging case described in the Smart Charging chapter of

Part 2 of OCPP, where a Local Controller can impose charge limits on its Charging Stations. In order for a Local Controller to be

addressed by the CSMS, it needs to have its own Charging Station identity. From the point of view from OCPP, the Local Controller

will just be a Charging Station (without any EVSEs/Connectors). The CSMS will possess the logic to deal with the Local Controller in

order to support, for example, local smart charging. It is up to the implementation of the CSMS, whether the group topology is

manually configured or deduced from the network based on IP addresses and information in BootNotifications. The diagram below

illustrate this configuration.

CSMS LC

Charging

Station 1

Charging

Station n

```
OCPP
```
```
OCPP
```
```
OCPP
```
_Figure 9. Multiple Charging Stations connected to CSMS via Local Controller_

**NOTE**

Technically this topology can be realized in multiple ways. When using this setup with websockets, this implies

that when a Charging Station connects to the Local Controller, it should open a websocket connection with the

same address to the CSMS. The advantages of this approach is that the Local Controller can see all the

messages and act on it, messages don’t have to wait, firmware updates etc. on the Charging Stations are

possible and the CSMS does not need special software. It could (in big installations) lead to a lot of websocket

connections between CSMS and LC needed. For further information, please refer to OCPP implementation guide

in Part 4.

### 8.4. Non-OCPP Charging Stations connected to CSMS via OCPP Local Controller.

**Description**

This setup has multiple non-OCPP Charging Stations that are abstracted away using a OCPP enabled Local Controller. When

applying OCPP in this situation, the LC should be considered as a Charging Station with many EVSEs or the LC should act as

multiple OCPP Charging Stations (having their own Charging Station Identity).

```
CSMS LC
```
```
Charging
```
```
Station 1
```
```
Charging
```
```
Station n
```
```
OCPP
```
```
non-OCPP
```
```
non-OCPP
```
_Figure 10. Multiple non-OCPP Charging Stations connected to CSMS via Local Controller_

### 8.5. DSO control signals to CSMS

**Description**

This is a set up in which the CSMS is the only application sending signals to a its Charging Stations, but the CSMS receives smart

charging signals from a DSO based on (most likely) grid constraints. This means that a non-OCPP signal such as OpenADR or

OSCP is received and based on this signal, the CSMS limits charging on its Charging Stations. CSOs that want full control over their

Charging Station use this architecture, this way they are in control of the amount of energy being used by their Charging Stations.

This can be done by sending charging profiles / charging schedules to Charging Stations.

```
DSO CSMS
```
```
Charging
```
```
Station
```
```
Non-OCPP (e.g. OpenADR or OSCP) OCPP
```
_Figure 11. Smart Charging - DSO control signals to CSMS_


### 8.6. Parallel control by CSMS and EMS.

**Description**

In a (semi-)private situation where a Charging Station is not only connected to the CSMS, but also to an Energy Management

System, some form of parallel control should be supported. OCPP should at least be used for Charging Station maintenance, but

OCPP 2.0.1 also supports reporting external smart charging control limits. So if the Energy Management System decides that

charging at a later time is "better", the Energy Management System can impose an external limit (e.g. 0) to a Charging Station,

which the Charging Station in turn can report to the CSMS via OCPP. The Energy Management System might get input from e.g.

Local port of Smart Meter to prevent overloading connection but can also have other reasons for not charging (e.g. weather

conditions).

CSMS EMS

Charging

Station

OCPP OCPP or other

_Figure 12. Parallel control by CSMS and EMS_


## 9. Part 1 Appendix: OCPP Information Model.

### 9.1. Explanation of UML representation and message generation

In the next paragraph, the UML schemes of the OCPP Information Model are shown. The model is based on the Common

Information Model (CIM) and to some extent to the CEFACT naming standards (only part of the standard). The objects in the model

are named _BusinessComponents_ and inherit properties from the CIM _IdentifiedObject_ , such as MRID and Name. In the UML

diagrams the attributes that are inherited from _IdentifiedObject_ are shown under the _IdentifiedObject_ stereotype (between < < > >).

Other attributes are listed under the stereotype < < Content > >.

The messages in OCPP are derived from the model represented in the next paragraph, in a 3 step process:

Information Model Message Model

Specification

Schemas

_Figure 13. Process from information Model to Messages / schemes_

After creating the Information Model, the messages are created based on the Information Model. However, in this transition (first

arrow), some rules are (manually) applied for modelling messages. The most important rule that is applied, is that messages

containing a reference to a <class> with only one <field>, are replaced by a field with the name <class><field>. For example, if a

message contains a Transaction, with only an Id, this is replaced by a transactionId.

In the next step, when generating the messages and datatypes section of Part 2 of the specification, for readability, all Core

DataTypes such as _CounterType_ , are replaced by the Primitive DataType they refer to (except for enumerations) in this example

_integer_.


### 9.2. Visual Representation of OCPP Information Model

```
«BusinessComponent»
```
```
Components::IdentifiedObject
```
```
«Content»
```
```
+ MRID: NumericIdentifierType [ 0 .. 1 ]
```
```
+ ObjectID: IdentifierType [ 0 ..*]
```
```
+ Name: NameType [0..1]
```
```
+ AliasName: NameType [0..*]
```
```
+ Description: TextType [ 0 ..*]
```
```
DeviceType
```
```
«BusinessComponent»
```
```
RootModel::EVSEType
```
```
+ ConnectorId: NumericIdentifierType [ 0 .. 1 ]
```
```
«Content»
```
```
+ Switch3To1PhaseSupported: IndicatorType [0..1]
```
```
«BusinessComponent»
```
```
TransactionType
```
```
«Content»
```
```
+ SeqNo: CounterType [0..1]
```
```
+ ChargingState: ChargingStateEnumType [0..1]
```
```
+ Offline: IndicatorType [ 0 .. 1 ]
```
```
+ StartedTimestamp: DateTimeType [0..1]
```
```
+ StoppedTimestamp: DateTimeType [0..1]
```
```
+ TimeSpentCharging: SecondsType [0..1]
```
```
+ StoppedReason: ReasonEnumType [0..1]
```
```
+ NumberOfPhasesUsed: CounterType [0..1]
```
```
+ MaxAllowedHours: SecondsType [0..1]
```
```
+ MaxAllowedKwh: PowerType [0..1]
```
```
+ TriggerReason: TriggerReasonEnumType [0..1]
```
```
+ RemoteStartID: RequestIdType [0..1]
```
```
+ Id: TransactionIdType [ 0 .. 1 ]
```
```
+ RunningCost: AmountType [0..1]
```
```
+ TotalCost: AmountType [0..1]
```
```
«BusinessComponent»
```
```
SmartCharging::ChargingProfileType
```
```
+ TransactionId: TransactionIdType [ 0 .. 1 ]
```
```
«Content»
```
```
+ StackLevel: CounterType [0..1]
```
```
+ Primary: IndicatorType [0..1]
```
```
+ TimeBase: DateTimeType [0..1]
```
```
+ ChargingProfilePurpose:
```
```
ChargingProfilePurposeEnumType [ 0 .. 1 ]
```
```
+ ChargingProfileKind: ChargingProfileKindEnumType
```
#### [0..1]

```
+ RecurrencyKind: RecurrencyKindEnumType [0..1]
```
```
+ ValidFrom: DateTimeType [0..1]
```
```
+ ValidTo: DateTimeType [0..1]
```
```
+ ChargingLimitSource: ChargingLimitSourceEnumType
```
#### [0..*]

```
«BusinessComponent»
```
```
Components::MessageInfoType
```
```
«Content»
```
```
+ Priority: MessagePriorityEnumType [0..1]
```
```
+ State: MessageStateEnumType [0..1]
```
```
+ StartDateTime: DateTimeType [0..1]
```
```
+ EndDateTime: DateTimeType [0..1]
```
```
«BusinessComponent»
```
```
Metering::SampledValueType
```
```
«Content»
```
```
+ Value: MeasureType [0..1]
```
```
+ Context: ReadingContextEnumType [0..1]
```
```
+ Measurand: MeasurandEnumType [0..1]
```
```
+ Phase: PhaseEnumType [0..1]
```
```
+ Location: LocationEnumType [ 0 .. 1 ]
```
```
«BusinessComponent»
```
```
ReservationType
```
```
«Content»
```
```
+ CreationTimestamp: DateTimeType [ 0 .. 1 ]
```
```
+ ExpiryDateTime: DateTimeType [0..1]
```
```
::IdentifiedObject
```
```
+ MRID: NumericIdentifierType [ 0 .. 1 ]
```
```
+ ObjectID: IdentifierType [ 0 ..*]
```
```
+ Name: NameType [0..1]
```
```
+ AliasName: NameType [0..*]
```
```
+ Description: TextType [ 0 ..*]
```
```
«BusinessComponent»
```
```
Security::IDTokenInfoType
```
```
«Content»
```
```
+ Status: AuthorizationStatusEnumType [ 0 .. 1 ]
```
```
+ CacheExpiryDateTime: DateTimeType [0..1]
```
```
+ Language1: LanguageType [0..1]
```
```
+ Language2: LanguageType [0..1]
```
```
+ ChargingPriority: NumericIdentifierType [ 0 .. 1 ]
```
```
+ ListID: IdentifierType [ 0 .. 1 ]
```
```
+ ListEntryTimestamp: DateTimeType [0..1]
```
```
DeviceFunctionType
```
```
«BusinessComponent»
```
```
Metering::MeteringFunctionType
```
```
«BusinessComponent»
```
```
RootModel::EVType
```
```
«BusinessComponent»
```
```
Security::IDTokenType
```
```
+ IDToken: TokenType [0..1]
```
```
+ Type: IDTokenEnumType [0..1]
```
```
DeviceType
```
```
«BusinessComponent»
```
```
RootModel::ConnectorType
```
```
«Content»
```
```
+ ConnectorType: ConnectorEnumType [0..1]
```
```
+ CableCapacity: CurrentType [0..*]
```
```
+TxnUpdateSampledValue
```
#### 0..*

```
+Reservation
```
#### 0..*

```
+GroupIDToken
```
#### 0..1

```
+Transaction
```
#### 0..*

```
+Connector
```
#### 0..1

```
+TxnStopSampledValue
```
#### 0..*

```
+Transaction
```
#### 0..1

#### +EV

#### 0..1

```
+TxnStartSampledValue
```
#### 0..*

#### +EV 0..*

```
+ChargingProfile 0..*
```
```
+Message
```
#### 0..*

```
+Transaction
```
#### 0..1

```
+Transaction 0..1
```
```
+Reservation
```
#### 0..1

```
+Reservation 0..*
```
```
+Connector 0..1
```
```
+Reservation 0..*
```
#### +EVSE 0..1

```
+Reservation
```
#### 0..*

```
+IDToken
```
#### 0..1

```
+Transaction
```
#### 0..*

#### +EVSE

#### 0..1

```
+Transaction 0..1
```
```
+IDToken 0..*
```
#### +EVSE

#### 0..1

```
+ChargingProfile 0..*
```
```
+Transaction
```
#### 0..*

```
+MeteringFunction
```
#### 0..*

```
+IDTokenInfo 0..1
```
```
+IDToken 0..1
```
```
+ChargingProfile
```
#### 0..1

```
+Transaction
```
#### 0..1

_Figure 14. OCPP Information Model: Transactions_


```
«BusinessComponent»
```
```
RootModel::EVCCType
```
```
«Content»
```
```
+ EMAID: ChargingContractIdentifierType [ 0 .. 1 ]
```
```
+ ContractSignatureEncryptedPrivateKey: SmallBinaryObjectType [0..1]
```
```
+ DHPublicKey: PublicKeyType [0..1]
```
```
+ Signature: SignatureType [0..1]
```
```
IdentifiedObject
```
```
«BusinessComponent»
```
```
Security::CertificateChainType
```
```
«Content»
```
```
+ Certificate: Base 64 String 800 Type [ 0 .. 1 ]
```
```
+ CertificateSerial: CI 20 TextType [ 0 .. 1 ]
```
```
+ ExpiryDate: DateType [0..1]
```
```
::IdentifiedObject
```
```
+ MRID: NumericIdentifierType [ 0 .. 1 ]
```
```
+ ObjectID: IdentifierType [ 0 ..*]
```
```
+ Name: NameType [0..1]
```
```
+ AliasName: NameType [0..*]
```
```
+ Description: TextType [ 0 ..*]
```
```
DeviceType
```
```
«BusinessComponent»
```
```
RootModel::EVSEType
```
```
+ ConnectorId: NumericIdentifierType [ 0 .. 1 ]
```
```
«Content»
```
```
+ Switch3To1PhaseSupported: IndicatorType [0..1]
```
```
DeviceType
```
```
«BusinessComponent»
```
```
RootModel::ConnectorType
```
```
«Content»
```
```
+ ConnectorType: ConnectorEnumType [0..1]
```
```
+ CableCapacity: CurrentType [0..*]
```
```
«BusinessComponent»
```
```
RootModel::EVType
```
```
«BusinessComponent»
```
```
ChargingNeedsType
```
```
«Content»
```
```
+ RequestedEnergyTransfer: EnergyTransferModeEnumType [0..1]
```
```
+ DepartureTime: DateTimeType [0..1]
```
```
«BusinessComponent»
```
```
ACChargingParametersType
```
```
«Content»
```
```
+ EnergyAmount: EnergyAmountType [0..1]
```
```
+ EVMinCurrent: CurrentType [0..1]
```
```
+ EVMaxCurrent: CurrentType [0..1]
```
```
+ EVMaxVoltage: VoltageType [0..1]
```
```
«BusinessComponent»
```
```
DCChargingParametersType
```
```
«Content»
```
```
+ EVMaxCurrent: NumericIdentifierType [ 0 .. 1 ]
```
```
+ EVMaxVoltage: NumericIdentifierType [ 0 .. 1 ]
```
```
+ EnergyAmount: NumericIdentifierType [ 0 .. 1 ]
```
```
+ EVMaxPower: NumericIdentifierType [ 0 .. 1 ]
```
```
+ StateOfCharge: PercentageType [0..1]
```
```
+ EVEnergyCapacity: NumericIdentifierType [ 0 .. 1 ]
```
```
+ FullSoC: PercentageType [0..1]
```
```
+ BulkSoC: PercentageType [0..1]
```
```
IdentifiedObject
```
```
«BusinessComponent»
```
```
ChargingProfileType
```
```
+ TransactionId: TransactionIdType [ 0 .. 1 ]
```
```
«Content»
```
```
+ StackLevel: CounterType [0..1]
```
```
+ Primary: IndicatorType [0..1]
```
```
+ TimeBase: DateTimeType [0..1]
```
```
+ ChargingProfilePurpose: ChargingProfilePurposeEnumType [ 0 .. 1 ]
```
```
+ ChargingProfileKind: ChargingProfileKindEnumType [ 0 .. 1 ]
```
```
+ RecurrencyKind: RecurrencyKindEnumType [0..1]
```
```
+ ValidFrom: DateTimeType [0..1]
```
```
+ ValidTo: DateTimeType [0..1]
```
```
+ ChargingLimitSource: ChargingLimitSourceEnumType [0..*]
```
```
IdentifiedObject
```
```
«BusinessComponent»
```
```
ChargingScheduleType
```
```
«Content»
```
```
+ StartSchedule: DateTimeType [0..1]
```
```
+ Duration: SecondsType [ 0 .. 1 ]
```
```
+ ChargingRateUnit: ChargingRateUnitEnumType [0..1]
```
```
+ MinChargingRate: NumericType [0..1]
```
```
«BusinessComponent»
```
```
ChargingSchedulePeriodType
```
```
«Content»
```
```
+ StartPeriod: SecondsType [0..1]
```
```
+ Limit: MeasureType [0..1]
```
```
+ NumberPhases: CounterType [0..1]
```
```
+ PhaseToUse: NumericType [0..1]
```
```
IdentifiedObject
```
```
«BusinessComponent»
```
```
Transactions::TransactionType
```
```
«Content»
```
```
+ SeqNo: CounterType [0..1]
```
```
+ ChargingState: ChargingStateEnumType [0..1]
```
```
+ Offline: IndicatorType [ 0 .. 1 ]
```
```
+ StartedTimestamp: DateTimeType [0..1]
```
```
+ StoppedTimestamp: DateTimeType [0..1]
```
```
+ TimeSpentCharging: SecondsType [0..1]
```
```
+ StoppedReason: ReasonEnumType [0..1]
```
```
+ NumberOfPhasesUsed: CounterType [0..1]
```
```
+ MaxAllowedHours: SecondsType [0..1]
```
```
+ MaxAllowedKwh: PowerType [0..1]
```
```
+ TriggerReason: TriggerReasonEnumType [0..1]
```
```
+ RemoteStartID: RequestIdType [0..1]
```
```
+ Id: TransactionIdType [ 0 .. 1 ]
```
```
+ RunningCost: AmountType [0..1]
```
```
+ TotalCost: AmountType [0..1]
```
```
«BusinessComponent»
```
```
CompositeScheduleType
```
```
«Content»
```
```
+ Duration: SecondsType [ 0 .. 1 ]
```
```
+ ChargingRateUnit: ChargingRateUnitEnumType [0..1]
```
```
+ StartDateTime: DateTimeType [0..1]
```
```
DeviceType
```
```
«BusinessComponent»
```
```
RootModel::ChargingStationType
```
```
«BusinessComponent»
```
```
Security::X509IssuerSerialType
```
```
«Content»
```
```
+ X 509 IssuerName: IssuerIdentifierType [ 0 .. 1 ]
```
```
+ X509SerialNumber: IntegerType [0..1]
```
```
«BusinessComponent»
```
```
ChargingLimitType
```
```
«Content»
```
```
+ Limit: SingleFractionDigitNumericType [ 0 .. 1 ]
```
```
+ ChargingRateUnit: ChargingRateUnitEnumType [0..1]
```
```
+ ChargingLimitSource: ChargingLimitSourceEnumType [0..1]
```
```
+ IsGridCritical: IndicatorType [ 0 .. 1 ]
```
```
+CertificateChain
```
#### 0..*

```
+ParentCertificate 0 .. 1
```
#### +EVSE 0..1

```
+CompositeSchedule
```
#### 0..*

```
+ChargePoint 0..1
```
```
+ChargingProfile 0..*
```
#### +EVSE 0..1

#### +EV

#### 0..1

```
+ChargingProfile
```
#### 0..1

```
+Transaction
```
#### 0..1

```
+ChargingSchedule
```
#### 0..1

```
+ChargingNeeds 0..*
```
```
+DCChargingParameters
```
#### 0..1

```
+RootCertificate 0..*
```
```
+Transaction 0..1
```
#### +EV 0..1

#### +EV 0..1

#### +EVCC 0..1

```
+OEMProvisioningCertificate
```
#### 0..1

#### +EVSE 0..1

```
{ConnectorUse}
```
```
+Connector 0..* +EV
```
#### 0..1

```
+ChargingNeeds
```
#### 0..1

```
+ChildCertificate 0 ..*
```
#### +EVSE 0..*

```
+ChargingLimit
```
#### 0..1

```
+Connector0..1
```
#### +EV

#### 0..1

```
+ChargingSchedule0..*
```
```
+ChargingSchedulePeriod 0..*
```
```
+ContractSignatureCertificateChain 0..1
```
#### +EV

#### 0..*

```
+ChargingProfile
```
#### 0..*

```
+ChargingProfile 0..*
```
```
+ChargingSchedule 0..1
```
```
+SAProvisioningCertificateChain
```
#### 0..1

```
+Transaction
```
#### 0..*

```
+Connector 0..1
```
```
+ChargingNeeds
0..*
```
```
+ACChargingParameters
```
#### 0..1

```
+Transaction
```
#### 0..*

#### +EVSE 0..1

```
+ChargePoint0..1
```
```
+CompositeSchedule 0..*
```
_Figure 15. OCPP Information Model: SmartCharging_


```
DeviceType
```
```
«BusinessComponent»
```
```
MeterType
```
```
«Content»
```
```
+ Location: LocationEnumType [ 0 .. 1 ]
```
```
+ SupportedMeasurands: MeasurandEnumType [0..*]
```
```
«BusinessComponent»
```
```
MeterValueType
```
```
«Content»
```
```
+ Timestamp: DateTimeType [0..1]
```
```
«BusinessComponent»
```
```
MeteringFunctionType
```
```
IdentifiedObject
```
```
«BusinessComponent»
```
```
RootModel::DeviceFunctionType
```
```
«BusinessComponent»
```
```
SampledValueType
```
```
«Content»
```
```
+ Value: MeasureType [0..1]
```
```
+ Context: ReadingContextEnumType [0..1]
```
```
+ Measurand: MeasurandEnumType [0..1]
```
```
+ Phase: PhaseEnumType [0..1]
```
```
+ Location: LocationEnumType [ 0 .. 1 ]
```
```
«BusinessComponent»
```
```
ClockAlignedMeteringFunctionType
```
```
«Content»
```
```
+ ClockAlignedDataInterval: SecondsType [0..1]
```
```
+ MeterValuesAlignedData: MeasurandEnumType [0..*]
```
```
+ MeterValuesAlignedDataMaxLength: CounterType [0..1]
```
```
+ TxnStoppedAlignedData: MeasurandEnumType [0..*]
```
```
+ TxnStoppedAlignedDataMaxLength: CounterType [0..1]
```
```
«CDTComplexType,LDT,ECDMChange,...
```
```
«Supplementary»
```
```
+ measureUnit: String [0..1]
```
```
+ measureUnit.CodeListVersion: String [0..1]
```
```
(from CoreDataTypes)
```
```
«PRIMSimpleType»
```
```
(from PrimitiveDataTypes)
```
```
+ MeterValueSignature: Base64String2500Type [0..1]
```
```
+ SignatureMethod: SignatureMethodEnumType
```
```
+ EncodingMethod: EncodingMethodEnumType [0..1]
```
```
+ EncodedMeterValue: Base64String512Type [0..1]
```
```
«BusinessComponent»
```
```
UnitOfMeasureType
```
```
+ Unit: CI20TextType [0..1]
```
```
+ Multiplier: IntegerType [ 0 .. 1 ]
```
```
+MeteringFunction
```
#### 0..1

```
+MeterValue 0..*
```
```
+SampledValue
```
#### 0..1

```
+SignedMeterValue
```
#### 0..1

```
+Meter 0..1
```
```
+MeterValue 0..*
```
```
+UnitOfMeasure 0..1
```
```
+MeteringFunction
```
#### 0..1

```
+Meter 0..1
```
```
+SampledValue 0..*
```
_Figure 16. OCPP Information Model: Metering_


«BusinessComponent»

**VariableType**

+ Name: CI50TextType [0..1]

+ Instance: CI50TextType [0..1]

«BusinessComponent»

**VariableAttributeType**

+ Type: AttributeEnumType [ 0 .. 1 ] = Actual

+ Value: CI255TextType

+ Mutability: MutabilityEnumType [0..1]

+ Persistence: IndicatorType [0..1]

+ Constant: IndicatorType

«BusinessComponent»

**VariableCharacteristicsType**

+ Units: MeasurementUnitType

+ DataType: DataEnumType

+ MinLimit: IntegerType [0..1]

+ MaxLimit: IntegerType [0..1]

+ ValuesList: CI500TextType [0..1]

+ SupportsMonitoring: IndicatorType

«BusinessComponent»

**ComponentType**

+ Name: CI50TextType [0..1]

+ Instance: CI50TextType [0..1]

+ evse: NumericIdentifierType [ 0 .. 1 ]

+ connector: NumericIdentifierType [ 0 .. 1 ]

«BusinessComponent»

**VariableMonitoringType**

+ Value: MeasureType

+ Type: MonitorEnumType

+ Severity: SeverityEnumType

+ Cleared: IndicatorType [0..1] = false

+ Transaction: IndicatorType [ 0 .. 1 ]

+Variable

0..1

+VariableAttribute 0..*

+Variable 0..1

+VariableCharacteristics 0..*

+Variable 0..1

+Component 0..*

+VariableMonitoring1..*

+Variable

1

_Figure 17. OCPP Information Model: Device Model_


```
IdentifiedObject
```
```
«BusinessComponent»
```
```
SecurityProfileType
```
```
«Content»
```
```
::IdentifiedObject
```
```
+ MRID: NumericIdentifierType [ 0 .. 1 ]
```
```
+ ObjectID: IdentifierType [ 0 ..*]
```
```
+ Name: NameType [0..1]
```
```
+ AliasName: NameType [0..*]
```
```
+ Description: TextType [ 0 ..*]
```
```
«BusinessComponent»
```
```
BasicAuthenticationProfileType
```
```
«Content»
```
```
+ UserName: UserNameType [0..1]
```
```
+ Password: PasswordType [0..1]
```
```
«BusinessComponent»
```
```
TLSBasicAuthenticationProfileType
```
```
«BusinessComponent»
```
```
TLSDigestProfileType
```
```
«Content»
```
```
+ ChargePointCertificate: X 509 CertificateDigestType [ 0 .. 1 ]
```
```
+ CentralServerCertificate: X 509 CertificateDigestType [ 0 .. 1 ]
```
```
«BusinessComponent»
```
```
CertificateHierarchyType
```
```
«Content»
```
```
+ Type: CertificateHierarchyCodeType [ 0 .. 1 ]
```
```
IdentifiedObject
```
```
«BusinessComponent»
```
```
CertificateChainType
```
```
«Content»
```
```
+ Certificate: Base 64 String 800 Type [ 0 .. 1 ]
```
```
+ CertificateSerial: CI 20 TextType [ 0 .. 1 ]
```
```
+ ExpiryDate: DateType [0..1]
```
```
DeviceType
```
```
«BusinessComponent»
```
```
RootModel::ChargingStationType
```
```
«deprecated, Content»
```
```
+ ChargeBoxSerialNumber: SerialNumberType [0..1]
```
```
«Content»
```
```
+ EVSECount: CounterType [0..1]
```
```
IdentifiedObject
```
```
«BusinessComponent»
```
```
Components::FirmwareType
```
```
«Content»
```
```
+ Version: VersionType [0..1]
```
```
+ Status: FirmwareStatusEnumType [0..1]
```
```
+ Location: URIType [ 0 .. 1 ]
```
```
+ RetrieveDateTime: DateTimeType [0..1]
```
```
+ InstallDateTime: DateTimeType [0..1]
```
```
+ Signature: Base64String800Type [0..1]
```
```
+ ServiceURL: URIType [0..1]
```
```
«BusinessComponent»
```
```
X509IssuerSerialType
```
```
«Content»
```
```
+ X 509 IssuerName: IssuerIdentifierType [ 0 .. 1 ]
```
```
+ X509SerialNumber: IntegerType [0..1]
```
```
IdentifiedObject
```
```
«BusinessComponent»
```
```
Components::LogType
```
```
«Content»
```
```
+ Filename: FilenameType [0..1]
```
```
+ RemoteLocation: URIType [ 0 .. 1 ]
```
```
+ LogStartedDateTime: DateTimeType [0..1]
```
```
+ LogStoppedDateTime: DateTimeType
```
```
+ OldestTimestamp: DateTimeType [0..1]
```
```
+ LatestTimestamp: DateTimeType [0..1]
```
```
+RootCertificate 0..*
```
```
+CentralServerCertificate
```
#### 0..1

```
+FirmwareSigningCertificate
```
#### 0..1

```
+ChargePointCertificate
```
#### 0..1

```
+CentralServerCertificate
```
#### 0..1

```
+RootCertificate
```
#### 0..1

```
+ChargePointOperatorHierarchy
```
#### 0..1

```
+ChildCertificate 0 ..*
```
```
+ParentCertificate 0 .. 1
```
```
+ChargePoint 0..1
```
```
+SecurityProfile
```
#### 0..1

```
+CertificateChain 0..*
```
```
+ManufacturerHierarchy
```
#### 0..1

```
+SecurityLog
```
#### 0..1

_Figure 18. OCPP Information Model: Security-Profiles_


```
«BusinessComponent»
```
```
Components::IdentifiedObject
```
```
«Content»
```
```
+ MRID: NumericIdentifierType [ 0 .. 1 ]
```
```
+ ObjectID: IdentifierType [ 0 ..*]
```
```
+ Name: NameType [0..1]
```
```
+ AliasName: NameType [0..*]
```
```
+ Description: TextType [ 0 ..*]
```
```
«BusinessComponent»
```
```
AuthorizationListType
```
```
«Content»
```
```
+ VersionNumber: IntegerType [0..1]
```
```
+ MaxLength: CounterType [0..1]
```
```
+ MaxTransmitCount: CounterType [0..1]
```
```
+ LocalAuthListEnabled: IndicatorType [0..1]
```
```
+ LocalAuthListMaxLength: CounterType [0..1]
```
```
+ SendLocalListMaxLength: CounterType [0..1]
```
```
«BusinessComponent»
```
```
IDTokenInfoType
```
```
«Content»
```
```
+ Status: AuthorizationStatusEnumType [ 0 .. 1 ]
```
```
+ CacheExpiryDateTime: DateTimeType [0..1]
```
```
+ Language1: LanguageType [0..1]
```
```
+ Language2: LanguageType [0..1]
```
```
+ ChargingPriority: NumericIdentifierType [ 0 .. 1 ]
```
```
+ ListID: IdentifierType [ 0 .. 1 ]
```
```
+ ListEntryTimestamp: DateTimeType [0..1]
```
```
«enumeration,BDT...
```
```
CodeLists::
```
```
IDTokenEnumType
```
```
«enum, Facet»
```
```
Central
```
```
eMAID
```
```
ISO14443
```
```
ISO15693
```
```
Local
```
```
NoAuthorization
```
```
KeyCode
```
```
«BusinessComponent»
```
```
AuthorizationFunctionType
```
```
«Content»
```
```
+ AuthorizationListSupported: IndicatorType [ 0 .. 1 ]
```
```
+ LocalAuthorizationEnabled: IndicatorType [ 0 .. 1 ]
```
```
+ LocalAuthorizeOffline: IndicatorType [ 0 .. 1 ]
```
```
+ LocalPreAuthorize: IndicatorType [0..1]
```
```
+ AuthorizationCacheEnabled: IndicatorType [ 0 .. 1 ]
```
```
+ AllowOfflineTxForUnknownID: IndicatorType [ 0 .. 1 ]
```
```
+ AuthorizeRequestTxRequests: IndicatorType [0..1]
```
```
+ LawEnforcementGroupId: TokenType [0..1]
```
```
«BusinessComponent»
```
```
RootModel::DeviceFunctionType
```
```
«enumeration,BDTEnu...
```
```
CodeLists::
```
```
AuthorizationStatusEnumType
```
```
«enum, Facet»
```
```
Accepted
```
```
Blocked
```
```
Expired
```
```
Invalid
```
```
NoCredit
```
```
NotAllowedTypeEVSE
```
```
NotAtThisLocation
```
```
NotAtThisTime
```
```
ConcurrentTx
```
```
Unknown
```
```
«BusinessComponent»
```
```
Transactions::TransactionType
```
```
«Content»
```
```
+ SeqNo: CounterType [0..1]
```
```
+ ChargingState: ChargingStateEnumType [0..1]
```
```
+ Offline: IndicatorType [ 0 .. 1 ]
```
```
+ StartedTimestamp: DateTimeType [0..1]
```
```
+ StoppedTimestamp: DateTimeType [0..1]
```
```
+ TimeSpentCharging: SecondsType [0..1]
```
```
+ StoppedReason: ReasonEnumType [0..1]
```
```
+ NumberOfPhasesUsed: CounterType [0..1]
```
```
+ MaxAllowedHours: SecondsType [0..1]
```
```
+ MaxAllowedKwh: PowerType [0..1]
```
```
+ TriggerReason: TriggerReasonEnumType [0..1]
```
```
+ RemoteStartID: RequestIdType [0..1]
```
```
+ Id: TransactionIdType [ 0 .. 1 ]
```
```
+ RunningCost: AmountType [0..1]
```
```
+ TotalCost: AmountType [0..1]
```
```
«BusinessComponent»
```
```
Components::MessageContentType
```
```
«Content»
```
```
+ Format: MessageFormatEnumType [0..1]
```
```
+ Language: LanguageType [0..1]
```
```
+ Content: MessageType [0..1]
```
```
«BusinessComponent»
```
```
IDTokenType
```
```
+ IDToken: TokenType [0..1]
```
```
+ Type: IDTokenEnumType [0..1]
```
```
«BusinessComponent»
```
```
OCSPRequestDataType
```
```
+ HashAlgorithm: HashAlgorithmEnumType [0..1]
```
```
+ IssuerNameHash: IssuerNameType [0..1]
```
```
+ IssuerKeyHash: HashType [0..1]
```
```
+ SerialNumber: SerialNumberType [0..1]
```
```
+ ResponderURL: URIType [0..1]
```
```
+AuthorizationFunction
```
```
0..1
```
```
+Transaction
```
```
0..*
```
```
+AuthorizationCache
```
```
0..1
```
```
+AuthorizationList 0..1
```
```
+LocalAuthorization
```
```
0..*
```
```
+IDTokenInfo 0..1
```
```
+IDToken 0..1
```
```
+IDTokenInfo
```
```
0..*
```
```
+PersonalMessage
```
```
0..1
```
```
+AuthorizationList
```
```
0..*
```
```
+AuthorizationFunction
```
```
0..*
```
```
+OCSPCertificateHash 0..*
```
```
+Transaction 0..1
```
```
+IDToken
```
```
0..*
```
```
+IDTokenInfo
```
```
0..*
```
```
+TariffMessage
```
```
0..1
```
_Figure 19. OCPP Information Model: Security-Authorization_


```
«BusinessComponent»
```
```
RootModel::DeviceFunctionType
```
```
«BusinessComponent»
```
```
NetworkConnectionProfileType
```
```
«Content»
```
```
+ Priority: IntegerType [0..1]
```
```
+ HeartbeatInterval: SecondsType [0..1]
```
```
+ OcppVersion: OCPPVersionCodeType [0..1]
```
```
+ OcppTransport: OCPPTransportCodeType [0..1]
```
```
+ OcppCsmsUrl: URIType [0..*]
```
```
+ OcppInterface: OCPPInterfaceCodeType [0..1]
```
```
«BusinessComponent»
```
```
ModemType
```
```
«Content»
```
```
+ ICCID: SimIdentifierType [ 0 .. 1 ]
```
```
+ IMSI: SimIdentifierType [ 0 .. 1 ]
```
```
«BusinessComponent»
```
```
APNType
```
```
«Content»
```
```
+ APN: URIType [0..1]
```
```
+ APNUserName: UserNameType [0..1]
```
```
+ APNPassword: PasswordType [0..1]
```
```
+ SimPin: PINEnumType [0..1]
```
```
+ PreferredNetwork: MobileNetworkIDType [0..1]
```
```
+ UseOnlyPreferredNetwork: IndicatorType [0..1]
```
```
+ APNAuthentication: APNAuthenticationCodeType [ 0 .. 1 ]
```
```
«BusinessComponent»
```
```
VPNType
```
```
«Content»
```
```
+ Server: URIType [0..1]
```
```
+ User: UserNameType [0..1]
```
```
+ Group: GroupNameType [0..1]
```
```
+ Password: PasswordType [0..1]
```
```
+ Key: VPNKeyType [0..1]
```
```
+ Type: VPNCodeType [0..1]
```
```
«BusinessComponent»
```
```
Components::IdentifiedObject
```
```
«Content»
```
```
+ MRID: NumericIdentifierType [ 0 .. 1 ]
```
```
+ ObjectID: IdentifierType [ 0 ..*]
```
```
+ Name: NameType [0..1]
```
```
+ AliasName: NameType [0..*]
```
```
+ Description: TextType [ 0 ..*]
```
```
«BusinessComponent»
```
```
TriggerType
```
```
«Content»
```
```
+ TriggerCode: MessageTriggerEnumType [0..1]
```
```
+VPN 0..1
```
```
+CommunicationFunction 0..*
```
```
+CommunicationModule 0..1
```
```
+APN 0..1
```
```
+Receiver
```
```
0..*
```
```
+Trigger
```
```
0..*
```
_Figure 20. OCPP Information Model: Communication_


«BusinessComponent»

**Components::IdentifiedObject**

«Content»

+ MRID: NumericIdentifierType [ 0 .. 1 ]

+ ObjectID: IdentifierType [ 0 ..*]

+ Name: NameType [0..1]

+ AliasName: NameType [0..*]

+ Description: TextType [ 0 ..*]

«BusinessComponent»

**CostType**

«Content»

+ CostKind: CostKindCodeType [0..1]

+ Amount: AmountType [0..1]

+ AmountMultiplier: IntegerType [ 0 .. 1 ]

«BusinessComponent»

**ConsumptionCostType**

«Content»

+ StartValue: NumericType [0..1]

«BusinessComponent»

**PMaxScheduleType**

«Content»

+ PMax: PowerType [0..1]

«BusinessComponent»

**RelativeTimeIntervalType**

«Content»

+ Start: SecondsType [0..1]

+ Duration: SecondsType [ 0 .. 1 ]

«BusinessComponent»

**SalesTariffType**

«Content»

+ SalesTariffDescription: TariffDescriptionType [ 0 .. 1 ]

+ NumEPriceLevels: CounterType [0..1]

«BusinessComponent»

**SAScheduleType**

«BusinessComponent»

**SalesTariffEntryType**

+ EPriceLevel: UnsignedIntegerType [0..1]

0..1

+RelativeTimeInterval

0..1

+Cost 0..*

+SASchedule 0..*

+SalesTariff

0..1

0..*

+ConsumptionCost 0..3

+RelativeTimeInterval

0..1

0..1

+SalesTariffEntry 1..1024

_Figure 21. OCPP Information Model: SecondaryActorSchedule_

