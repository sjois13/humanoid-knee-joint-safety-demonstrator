# Humanoid Knee Joint Safety Demonstrator

Safety architecture study for one load-bearing knee joint of an industrial humanoid robot.

The project follows one hazardous event from system definition through safety requirements, failure analysis, fault reaction and verification planning:

**HE-01 — Unintended or excessive knee motion while the humanoid is load-bearing and operating close to a person.**

📄 [Download the complete PDF portfolio](pdf/Humanoid_Knee_Joint_Safety_Architecture.pdf)

## Standards Context

The safety concept is developed with reference to machinery, robotics and drive functional-safety principles, including:

- **ISO 10218** — industrial robot safety
- **ISO 13849-1** — safety-related control systems and Performance Level concepts
- **IEC 61800-5-2** — safety-related drive functions such as SLS, SS1 and STO
- **IEC 61508** — general functional-safety principles

The current project does not claim compliance or certification to these standards.

A provisional ISO 13849-style risk assessment is included for the selected operating scenario. Under the stated S/F/P assumptions, **PLr d** is used as a preliminary target for further development.

No achieved Performance Level or ISO 13849 architecture category is claimed.

The fault-reaction concept also uses IEC 61800-5-2 terminology where useful:

- **FSR-11 — Independent Safety Envelope Monitoring** is conceptually related to functions such as Safely-Limited Speed (SLS).
- **FSR-09 — Independent Safety Reaction Path** provides the architectural basis for torque inhibition similar to STO-type functionality.
- Controlled stopping concepts are compared with SS1 where the normal control path remains trustworthy.

These are conceptual mappings only. The corresponding safety functions have not been implemented or validated as certified drive functions.

## What is Covered

- system boundary and operating assumptions
- knee-joint control and actuation architecture
- hazard analysis and provisional risk assessment
- safety goal and functional safety requirements
- focused FMEA and qualitative fault tree
- safety monitoring and independent fault reaction
- requirements-to-architecture traceability
- verification and fault-injection planning
- reaction-time concept

## Main Design Idea

The knee is treated as a local safety-critical actuator.

The robot-level controller defines the intended motion, while the local knee system executes, monitors and constrains the actuator.

The safety architecture uses both:

- command-versus-response monitoring
- command-independent safety limits

This is important because an unsafe command may be executed correctly without producing a command-to-response deviation.

Another key point is that torque removal is not automatically a complete safe reaction for a load-bearing joint.

If actuator torque is removed, the knee may lose its ability to support the robot. The safety concept therefore separates **torque inhibition** from **load holding** and selects the reaction based on which control and sensing functions remain trustworthy.

## Documents

- [01 — System Definition](docs/01_System_Definition.md)
- [02 — System Architecture](docs/02_System_Architecture.md)
- [03 — Hazard Analysis](docs/03_Hazard_Analysis.md)
- [04 — Safety Requirements](docs/04_Safety_Requirements.md)
- [05 — FMEA and Fault Tree Analysis](docs/05_FMEA_FTA.md)
- [06 — Safety Concept, Traceability and Verification](docs/06_Safety_Concept_Traceability_VV.md)

Supporting files:

- [`analysis/`](analysis/) — focused FMEA and supporting analysis
- [`diagrams/`](diagrams/) — architecture and safety diagrams

## Current Status

The current work is a system-level safety demonstrator.

The architecture, hazard analysis, safety requirements, FMEA, FTA, fault-reaction concept and verification plan are defined.

The following are still open:

- numerical safety limits and diagnostic thresholds
- reaction-time budget
- Safety Monitor independence
- detailed torque-inhibit hardware
- load-holding / brake design
- quantitative diagnostic coverage
- common-cause analysis
- achieved ISO 13849 category / Performance Level
- quantitative reliability analysis
- physical verification evidence

A small fault-injection simulation is planned to verify velocity-deviation monitoring, independent safety-envelope monitoring and diagnostic reaction time.

## Tools and Development

The project was developed through system-level safety analysis, architecture work and iterative review.

AI-assisted tools were used for documentation support and review. Engineering assumptions, architecture decisions, safety requirements and open technical points are kept explicit in the project documentation.
