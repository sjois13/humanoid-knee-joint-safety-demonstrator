# Humanoid Knee Joint Safety Demonstrator

Safety analysis and architecture study for one load-bearing knee joint of an industrial humanoid robot.

The project follows the chain from system definition to verification for one selected hazardous event:

**HE-01 — Unintended or excessive knee motion while the humanoid is load-bearing and operating close to a person.**

## What is covered

- system boundary and assumptions
- joint control and actuation architecture
- hazard analysis and safety goal
- functional safety requirements
- focused FMEA and fault tree
- fault-reaction concept
- requirements traceability
- verification planning

## Main design idea

The knee joint is treated as a local safety-critical actuator.

The robot-level controller decides the intended motion, while the local joint system executes, monitors and constrains the actuator.

A key point in the safety concept is that immediate torque removal is not always the safest reaction for a load-bearing joint. Fault reaction depends on which control and sensing functions remain trustworthy.

## Documents

- [01 — System Definition](docs/01_System_Definition.pdf)
- [02 — System Architecture](docs/02_System_Architecture.pdf)
- [03 — Hazard Analysis](docs/03_Hazard_Analysis.pdf)
- [04 — Safety Requirements](docs/04_Safety_Requirements.pdf)
- [05 — FMEA and FTA](docs/05_FMEA_FTA.pdf)
- [06 — Safety Concept, Traceability and V&V](docs/06_Safety_Concept_Traceability_V&V.pdf)

Supporting files:

- [`analysis/`](analysis/) — focused FMEA
- [`diagrams/`](diagrams/) — architecture and fault-tree figures

## Status

The current work is a qualitative system-level demonstrator.

Diagnostic thresholds, reaction times, quantitative reliability analysis and physical verification evidence are not yet developed.

## Tools

Engineering content was developed through system-level analysis, study and iteration. 
AI tools were used purely as a support for documentation structure and presentation purposes only.
