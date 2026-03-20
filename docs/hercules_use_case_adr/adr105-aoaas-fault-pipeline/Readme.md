---
id: fx_adr105
title: ADR 105 – Fault Diagnosis & Correction
date: 2026-03-12
tags:
  - architecture_decision_records
  - API
---

## Purpose

This Architecture Decision Record (ADR) provides normative guidance for the use case **"Autonomous Operation as a Service (AOaaS)"**. As **"Autonomous Operation as a Service"** provides different scenarios, the ADRs are separated.

This ADR provides guidance about **Fault Diagnosis and Correction**.

Autonomous Operation as a Service aims at keeping machine downtime minimal by accelerating the process to solve a faulty situation using knowledge of prior events as well as human ingenuity.
One of the main "fault-to-solution" paths within AOaaS involves a Machine Operator diagnosing faulty situations remotely while only using machine data and video information.

The main chain of thought here is:

- Something goes wrong, the situation is transmitted to a remote operator
- the remote operator might gather further information and fault descriptions from vendors and suppliers
- the remote operator uses this information to deduct possible corrections and documents them
- the corrections are recommended/executed

The provided machine data as well as the basic procedure need to be presented in a standardized format, which is defined in this document.

## Roles

[TODO: I wanted to extend this a little, as it might not be clear to everyone - and to just have space for clarification on Betreiber vs Besitzer vs Benutzer blabla]

### _Fault Data Provider_

This is the party that actually "has the faulty situation", usually the **Machine Owner** or the **Machine User**.

> Note: This document also defines the `Remote Operator`. Do avoid confusion, the `Machine User` represents the party that actually uses the machine, although _Machine Operator_ might be the more correct description.

### _Fault Resolution Expert_

This party has additional information about the machine and its components, which might not be available to the _Fault Data Provider_.
This could be the **Machine Builder** or one of many **Component Suppliers**.

### _Solution Provider_

This party offers a service to solve the faulty situation for a `Fault Data Provider`.
For the case under consideration within this document that would be a **Machine (Remote) Operator**, or **Remote Operator** for short.

## Definitions

[TODO: Just an idea, as these words might not be defined the same in everyones head - could be removed if not needed]

#### Symptom

[TODO: Briefly describe what a Symptom is in this context]

#### (Faulty) Situation

[TODO: Just to clearly define what a "situation" is mostly]

## API Structure

### Fault Data Provider

The `Fault Data Provider` MUST expose the endpoints according to the following Architecture Decision Records (ADRs):

| Architecture Decision Record (ADR)                                       | Version | Link                                                                                                                      |
| ------------------------------------------------------------------------ | ------- | ------------------------------------------------------------------------------------------------------------------------- |
| ADR 002 – Cross-Company Authorization and Discovery Version 0.2.0        | 0.2.0   | https://factory-x-contributions.github.io/architecture-decisions/docs/hercules_network_adr/adr002-authorization-discovery |
| ADR 003 – Authentication for Dataspaces Version 0.2.0                    | 0.2.0   | https://factory-x-contributions.github.io/architecture-decisions/docs/hercules_network_adr/adr003-authentication          |
| ADR 008 – Asset Administration Shell Profile for Factory-X Version 0.2.0 | 0.2.0   | https://factory-x-contributions.github.io/architecture-decisions/docs/hercules_network_adr/adr008-aas-profile             |
| ADR 009 – Discovery of AAS Services via DSP Version 0.2.0                | 0.2.0   | https://factory-x-contributions.github.io/architecture-decisions/docs/hercules_network_adr/adr009-aas-rest-dsp            |

[TODO: Editor Note: (see also Editor Note on the Solution Provider) - to be discussed: Should the Fault Data provider not just have some sort of client to connect to the broker of a solution provider? Would that mean that one solution provider has to be connected to many Fault Data Providers in order to receive notifications about new situations? As of now, I put the broker on the Solution Provider - please revert if that is wrong]

> Note: This would also contain video data as defined in ADR104 if needed

[TODO: Remove this next note if other means of mqtt-over-dsp clients are covered]

> Note: The `Fault Data Provider` is also expected to connect to MQTT Brokers provided by a `Solution Provider` according to the relevant ADRs described in the `Solution Provider` section below

### Fault Resolution Expert

The `Fault Resolution Expert` MUST expose the endpoints according to the following Architecture Decision Records (ADRs):

| Architecture Decision Record (ADR)                                       | Version | Link                                                                                                                      |
| ------------------------------------------------------------------------ | ------- | ------------------------------------------------------------------------------------------------------------------------- |
| ADR 002 – Cross-Company Authorization and Discovery Version 0.2.0        | 0.2.0   | https://factory-x-contributions.github.io/architecture-decisions/docs/hercules_network_adr/adr002-authorization-discovery |
| ADR 003 – Authentication for Dataspaces Version 0.2.0                    | 0.2.0   | https://factory-x-contributions.github.io/architecture-decisions/docs/hercules_network_adr/adr003-authentication          |
| ADR 008 – Asset Administration Shell Profile for Factory-X Version 0.2.0 | 0.2.0   | https://factory-x-contributions.github.io/architecture-decisions/docs/hercules_network_adr/adr008-aas-profile             |
| ADR 009 – Discovery of AAS Services via DSP Version 0.2.0                | 0.2.0   | https://factory-x-contributions.github.io/architecture-decisions/docs/hercules_network_adr/adr009-aas-rest-dsp            |

> Note: Think of it as an AAS database for further information that only a **Machine Builder** or **Component Supplier** might have

### Solution Provider

The `Solution Provider` MUST expose the endpoints according to the following Architecture Decision Records (ADRs):

| Architecture Decision Record (ADR)                                | Version | Link                                                                                                                         |
| ----------------------------------------------------------------- | ------- | ---------------------------------------------------------------------------------------------------------------------------- |
| ADR 002 – Cross-Company Authorization and Discovery Version 0.2.0 | 0.2.0   | https://factory-x-contributions.github.io/architecture-decisions/docs/hercules_network_adr/adr002-authorization-discovery    |
| ADR 003 – Authentication for Dataspaces Version 0.2.0             | 0.2.0   | https://factory-x-contributions.github.io/architecture-decisions/docs/hercules_network_adr/adr003-authentication             |
| ADR 011 – Eventing with AAS Payloads Version 0.2.0                | 0.2.0   | https://factory-x-contributions.github.io/architecture-decisions/docs/hercules_network_adr/adr011-eventing-with-aas-payloads |
| ADR 012 – MQTT over DSP Version 0.2.0                             | 0.2.0   | https://factory-x-contributions.github.io/architecture-decisions/docs/hercules_network_adr/adr012-mqtt-over-dsp              |

[TODO: Editor Note: Not sure about this one, as this is probably a matter to be discussed: I think the solution provider should hold the
actual broker for mqtt-over-dsp, whereas the Fault Data Provider only needs a mqtt-over-dsp client of some sort]

> Note: The `Solution Provider` is also expected to write to AAS data of the `Fault Data Provider` to provide solutions

## Data Models

The following submodels are required:

| Submodel            | Version | Reference                       | Status   | Role                    |
| ------------------- | ------- | ------------------------------- | -------- | ----------------------- |
| Situation Log       | 1.0     | [AOaaS Submodel Template (TBD)] | Required | Fault Data Provider     |
| Fault Description   | 1.0     | [AOaaS Submodel Template (TBD)] | Required | Fault Resolution Expert |
| Similarity Analysis | 1.0     | [AOaaS Submodel Template (TBD)] | Required | Solution Provider       |
| Fault Correction    | 1.0     | [AOaaS Submodel Template (TBD)] | Required | Solution Provider       |

[TODO: Mostly so we do not forget about it: Provide templates or at least basic examples for every submodel mentioned above or mark them as not finished if they cannot be provided at this point yet]

The following submodels might be also used (mostly to provide additional information to the remote machine operator):

| Submodel                                           | Version | Reference                                                                                                                                                       | Status   | Role                                         |
| -------------------------------------------------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | -------------------------------------------- |
| Asset Interfaces Description                        | 1.0     | [IDTA Submodel Template](https://github.com/admin-shell-io/submodel-templates/tree/main/published/Asset%20Interfaces%20Description)                             | Optional | Fault Data Provider                          |
| Time Series Data                                   | 1.1     | [IDTA Submodel Template](https://github.com/admin-shell-io/submodel-templates/tree/main/published/Time%20Series%20Data/1/1)                                     | Optional | Fault Data Provider                          |
| Handover Documentation                             | 2.0.1   | [IDTA Submodel Template](https://github.com/admin-shell-io/submodel-templates/tree/main/published/Handover%20Documentation/2/0/1)                               | Optional | Fault Data Provider, Fault Resolution Expert |
| Technical Data                                     | 2.0     | [IDTA Submodel Template](https://github.com/admin-shell-io/submodel-templates/tree/main/published/Technical_Data/2/0)                                           | Optional | Fault Data Provider, Fault Resolution Expert |
| Hierarchical Structures enabling Bills of Material | 1.1     | [IDTA Submodel Template](https://github.com/admin-shell-io/submodel-templates/tree/main/published/Hierarchical%20Structures%20enabling%20Bills%20of%20Material) | Optional | Fault Data Provider, Fault Resolution Expert |
| Capability Description                             | 1.0     | [IDTA Submodel Template](https://github.com/admin-shell-io/submodel-templates/tree/main/development/Capability/1/0)                                             | Optional | Fault Data Provider, Fault Resolution Expert |

> Note: Asset Interfaces Description might be used to get access to video data according to ADR 104

[TODO: When using mqtt-over-dsp, the message format should be defined, this just serves as an example]

The following message formats are required:

| Message Type           | Reference                     | Status   | Sender              | Receiver            |
| ---------------------- | ----------------------------- | -------- | ------------------- | ------------------- |
| NewSituationMessage    | [point to resources/msg.json] | required | Fault Data Provider | Solution Provider   |
| SituationResolveMesage | [point to resources/msg.json] | required | Solution Provider   | Fault Data Provider |

> Note: Situations are identified by a unique situationId [TODO: Just wrote this as I am not sure how identifiaction would actually work in a case where multiple situations would occur. If we do not want to include that, I think we should explicitly exclude that. but that is also a discussion topic]

### The Fault Solution Pipeline

The submodels are used in a basic process that has the following steps:

1. `Fault Data Provider`: A faulty situation within a system or machine is detected: A `Situation Log` is created.
2. The `Solution Provider` is notified using `mqtt-over-dsp` by receiving a `NewSituationMessage` from the `Fault Data Provider` [TODO: Just pulled the name out of thin air: This needs clear definition in the Data Models section above]
3. The `Solution Provider` investigates the `Situation Log` and retrieves `Fault Descriptions` for faulty components/machines from the `Fault Resolution Expert`
4. The `Solution Provider` uses `Situation Log`, `Fault Description` as well as any optional submodels from the list above to diagnose the faulty situation (also taking into effect previous situations and their solutions)
5. The `Solution Provider` captures his diagnosis within `Similarity Analysis` for future reference
6. The `Solution Provider` provides `Fault Correction` to solve the faulty situation to the `Fault Data Provider` [TODO: Did we specify anything about also playing this back to the Fault Resolution Expert? Can't remember. If not, just remove this TODO]
7. The `Solution provider` notifies the `Fault Data Provider` using `mqtt-over-dsp` by sending a `SituationResolvedMessage` using its own broker [TODO: Not sure about this one: Is there another way of notifying the Fault Data Provider about a finished procedure here?]
8. `Fault Data Provider`: Executes the `Fault Correction` to solve his faulty sitution

> Note: A `Solution Provider` may already have `Fault Descriptions` available and does not always need to retrieve this data from a `Fault Resolution Expert`

#### Situation Log

The `Situation Log` submodel is used to capture the context of a faulty situation by providing a set of all occured symptoms that were present within a certain timeframe around the time of fault detection.
One might think of it as a "symptom dashcam".

#### Fault Description

The `Fault Description` submodel contains detailed description of a fault that lead to a faulty situation. It also contains contextual information as well as references to associated symptoms (without duplicating data). Furthermore it contains references to associated `Fault Corrections`.

#### Similarity Analysis

[TODO: I am still not sure if I understand what exactly this Submodel would contain - guess the exmaple would be of big help here]

The `Similarity Analysis` submodel contains a comparison of various situations from the SituationLog with existing faults to determine the degree of similarity between the current situation and specific faults.

#### Fault Correction

[TODO: Might need clarification, at least for me: Is this purely created by The Fault Resolution Expert or can the operator also define them? I would guess that operators can, but I am not sure]
[TODO: Is this considered one Submodel per Faulty Situation or might there be a list of Fault Corrections for a Faulty Situation?]

The `Fault Correction` submodel contains recommended corrective actions, including predefined strategies and workflows, to address specific faults, with options for both automated and operator-initiated interventions.

#### Optional Submodels

- `Asset Interfaces Description` can be used to get access to streaming data according to ADR 104.
- `Time Series Data` can be used to gather any real-time and historical sensor and process data that might be of use to the remote operator.
- `Handover Documentation` can be used to understand system configuration and operational constraints and provide information about commissioning, setup and instructions.
- `Hierarchical Structures enabling Bills of Material` can be used to verify machine topology.
- `Technical Data` can be used to verify machine configurations, parameters and technical specifications.
- `Capabilities` can be used to indicate what the machine is capable of performing to derive appropriate operational strategies or actions, supporting flexible and autonomous operation.

## Authentication and Authorization

Apart from mechanism already described within the linked ADRs, no additional Authentication or Authorization mechanism is specified.
