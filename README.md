# Humanoid Knee Joint Safety Demonstrator

A personal system-safety study of one load-bearing knee joint of an industrial humanoid robot: hazard analysis, requirements, architecture, failure analysis and a fault-reaction concept.

**Status:** concept-level analysis. No hardware has been built and no test evidence exists yet. Open points are listed at the end.

**Author:** Sumukha Jois · [www.linkedin.com/in/sumukhajois]

## The question this study works through

**HE-01 — Unintended or excessive knee motion while the humanoid is load-bearing and operating close to a person.**

For a load-bearing joint, what is the safe state? Removing actuator torque stops commanded motion, but it also removes support, and the knee can collapse. The study follows that question from system definition through requirements, failure analysis, fault reaction and verification planning.

<!-- Add one diagram here, for example the reaction state machine or the architecture figure from diagrams/ -->

## Main design ideas

1. **Torque removal is not a complete safe reaction.** Torque inhibition and load holding are treated as separate functions. The reaction is chosen by which control and sensing functions remain trustworthy.
2. **Two kinds of monitoring.** Command-versus-response monitoring catches an actuator that does not do what it is told. Command-independent limits (speed, torque-producing current, position) catch a command that is unsafe but executed correctly, which shows no deviation at all.
3. **The safety path must be independent and supervised.** The Safety Monitor and the torque-inhibit path are kept separate from the normal controller, checked by self-test and watchdog, and analysed for shared resources (power, clock, processor).

## Preliminary safety functions

| ID | Safety function | Preliminary PLr |
|---|---|---|
| SF-01 | Limit hazardous knee velocity and torque-related actuation to the permitted envelope | d |
| SF-02 | Inhibit hazardous torque when normal control cannot be trusted | d |
| SF-03 | Keep the load-bearing knee mechanically supported after loss of active torque | d |

All three are preliminary and derived under the same S2 / F1 / P2 assumptions. Whether each one needs the same PLr is an open question.

## What is covered

- system boundary and operating assumptions
- knee-joint control and actuation architecture
- hazard analysis and provisional risk assessment
- safety goal and functional safety requirements
- focused FMEA and fault tree, including mechanical transmission faults
- safety monitoring and independent fault reaction
- requirements-to-architecture traceability
- verification and fault-injection planning

## Documents

- [01 — System Definition](docs/01_System_Definition.md)
- [02 — System Architecture](docs/02_System_Architecture.md)
- [03 — Hazard Analysis](docs/03_Hazard_Analysis.md)
- [04 — Safety Requirements](docs/04_Safety_Requirements.md)
- [05 — FMEA and Fault Tree Analysis](docs/05_FMEA_FTA.md)
- [06 — Safety Concept, Traceability and Verification](docs/06_Safety_Concept_Traceability_VV.md)

Supporting files: [`analysis/`](analysis/) (focused FMEA and supporting analysis), [`figures/`](figures/) (architecture and safety diagrams).

## Standards context

Two standards shape the concept directly:

- **ISO 13849-1:** a provisional risk assessment using the S/F/P risk graph. Under the stated assumptions, **PLr d** is used as a preliminary target. No achieved Performance Level and no architecture category is claimed.
- **IEC 61800-5-2:** drive safety-function terminology, used as a conceptual mapping:

| Concept in this study | Related drive safety function |
|---|---|
| Independent envelope monitoring (FSR-11) | SLS (safely-limited speed) |
| Controlled stop, then hold under active control | SS2 and SOS |
| Safe control of the holding brake | SBC (the function, not the mechanical brake) |
| Independent torque inhibition (FSR-09) | STO |

**ISO 10218** is the robot-level context for the human-proximity scenario; no mapping to it is attempted.

This project does not claim compliance or certification to any standard. The mappings are conceptual, and none of the functions has been implemented or validated.

## Open points

**Numbers and evidence**

- numerical safety limits and diagnostic thresholds
- reaction-time budget and the collapse-time estimate behind it
- fault-injection simulation (planned: velocity-deviation monitoring, envelope monitoring, diagnostic reaction time)
- physical verification evidence

**Design**

- Safety Monitor independence and detailed torque-inhibit hardware
- load-holding / brake design
- common-cause analysis

**Quantitative safety analysis**

- diagnostic coverage and reliability analysis
- achieved ISO 13849 category and Performance Level

## Tools and development

The project was developed through system-level safety analysis, architecture work and iterative review. AI-assisted tools were used for documentation support and review. Engineering assumptions, architecture decisions, safety requirements and open technical points are kept explicit in the project documentation.
