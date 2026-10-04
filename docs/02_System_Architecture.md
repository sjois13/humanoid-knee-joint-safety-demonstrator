# 2. System Architecture

## 2.1 Architecture Objective

The knee-joint architecture is developed around one main safety concern: unintended or excessive knee torque or motion while the joint is load-bearing.

The architecture supports normal closed-loop actuator control together with local monitoring and fault reaction.

A key design objective is that the same control path that can create hazardous torque should not be the only means available to detect or mitigate it.

Detailed circuit design, processor selection, motor electromagnetic design and production-level redundancy are outside the current scope.

## 2.2 Architecture Overview

![Figure 2 — Functional Architecture of the Knee Joint System](../figures/02_functional_architecture.png)

The architecture contains four main paths:

- command and control
- electrical and mechanical energy flow
- sensor feedback and diagnostics
- safety monitoring and fault reaction

The command path defines the requested actuator behaviour.

The energy path converts electrical power into physical knee torque and motion.

The feedback path provides information about the actual actuator state.

The safety path checks whether actuator behaviour remains within the expected and permitted operating envelope and initiates a reaction when required.

## 2.3 Command and Control Path

The normal command path is:

`Whole-Body Motion Controller → Communication Interface → Local Joint Controller → Motor Control / FOC → Gate Driver → Inverter`

The Whole-Body Motion Controller provides the required joint-level motion intent.

The Local Joint Controller executes this request subject to local limits and diagnostics.

Motor Control / FOC regulates motor current and generates PWM for the inverter.

**Allocation decision:** whole-body motion planning remains at robot level, while fast actuator execution, monitoring and local limitation remain within the knee system.

The local safety concept does not assume that every upstream command is correct.

## 2.4 Energy and Mechanical Actuation Path

The physical energy path is:

`Robot Power System → DC Bus → Three-Phase Inverter → PMSM Motor → Reduction Gearbox → Knee Joint`

This is the physical hazard-propagation path.

Faults in the power stage, motor or transmission can cause actual joint behaviour to differ from the intended behaviour. Monitoring software commands alone is therefore not sufficient.

## 2.5 Feedback and Diagnostic Path

The architecture uses measurements from several physical points in the actuator.

![Figure 3 — Feedback and Diagnostic Signals](../figures/03_feedback_diagnostic_signals.png)

| Signal | Used by | Architectural purpose |
| --- | --- | --- |
| Joint encoder | Local Controller, Safety Monitor | Actual knee motion |
| Motor encoder | Motor Control / FOC, Safety Monitor | Rotor state and motor-side motion |
| Motor current | Motor Control / FOC, Safety Monitor | Electrical actuation / torque indication |
| Voltage / temperature | Controller, Safety Monitor | Operating-condition supervision |

Some signals are used by both normal control and safety monitoring. This alone does not provide sensor independence.

Where the same sensor contributes to both functions, common sensor faults remain possible and must be considered in the safety analysis.

## 2.6 Cross-Monitoring Concept

Measurements from different physical points are compared to detect inconsistent actuator behaviour.

One example is:

`Motor Encoder ↔ expected transmission relationship ↔ Joint Encoder`

A mismatch may indicate:

- gearbox or coupling fault
- motor encoder fault
- joint encoder fault
- incorrect scaling or configuration

The check identifies that actuator behaviour is inconsistent; it does not necessarily identify which element has failed.

A similar check is used in the electrical path:

`Commanded Motor Current ↔ Measured Motor Current`

A persistent difference indicates that the electrical actuation path is not following the requested behaviour.

Cross-monitoring improves fault detection but does not by itself guarantee fault isolation or sensing independence. Additional sensing diversity may be required in a production design.

## 2.7 Safety Monitor

The Safety Monitor supervises selected safety-relevant relationships independently of the primary motion-control objective.

The main monitoring functions are:

- commanded-versus-measured joint-velocity monitoring
- commanded-versus-measured motor-current monitoring
- motor-to-joint transmission plausibility
- communication supervision
- sensor plausibility
- controller / timing supervision
- absolute actuator safety-envelope monitoring

The last function is important because command-to-response monitoring alone cannot detect an unsafe command that is executed correctly.

The Safety Monitor therefore also supervises command-independent limits such as:

- maximum permitted joint velocity
- permitted joint-position range
- maximum permitted torque / torque-producing current

The exact limits are not yet derived.

The Safety Monitor is not intended to be a duplicate joint controller. Its purpose is to reduce the possibility that one control-path failure can both create hazardous behaviour and prevent its detection.

The required implementation independence remains an open design item.

## 2.8 Independent Fault-Reaction Path

The architecture includes a hardware-capable path for inhibiting hazardous torque without relying solely on the normal control software.

For example:

`Local Joint Controller Fault → incorrect current request → hazardous motor torque`

If the same failed controller is also required to execute the only available shutdown command, the safety reaction contains a single dependency.

The independent path must therefore be capable of influencing torque generation separately from the normal controller.

For a load-bearing knee, torque inhibition alone may not provide a complete safe reaction. Loss of actuator torque can remove joint support and lead to collapse.

The final safety concept may therefore require torque inhibition to be combined with an independent load-holding mechanism or coordinated robot-level support.

The detailed hardware implementation is not defined in this demonstrator.

## 2.9 Robot-Level Safety Supervisor

Local actuator faults and degraded states are reported to the Robot Safety Supervisor.

The knee system is responsible for fast local detection and actuator-level reaction.

The Robot Safety Supervisor coordinates whole-body actions such as:

- posture recovery
- coordinated stopping
- degraded whole-body operation
- higher-level power isolation

This separation reflects the different information available at each level: the knee system has detailed actuator information, while the robot-level supervisor has the context required to coordinate the complete robot.

## 2.10 Fault-Reaction Philosophy

The fault reaction depends on which functions remain trustworthy after the fault.

If normal control remains trustworthy, possible reactions include:

- torque limitation
- velocity limitation
- degraded operation
- controlled stop

If normal control cannot be trusted, the reaction may require:

- independent torque inhibition
- power isolation
- independent load holding
- coordinated robot-level recovery

The guiding principle is:

**Use the least disruptive reaction that still provides a trustworthy means of controlling the hazard.**

## 2.11 Key Architecture Decisions

The main architecture decisions are:

- Whole-body motion planning remains outside the local knee boundary.
- Fast actuator execution and monitoring remain local to the knee.
- Motor-side and joint-side motion are measured separately.
- Motor current is treated as safety-relevant feedback.
- Safety monitoring includes both deviation monitoring and command-independent absolute limits.
- Safety monitoring is separated conceptually from normal control.
- Immediate torque removal is not treated as the universal safe reaction.
- Mechanical transmission faults remain part of the safety analysis.

## 2.12 Assumptions and Open Points

The following items require further development:

- PMSM and three-phase inverter are assumed as the actuator technology.
- Approximately 48 V is used as a representative actuator DC bus, not as a derived requirement.
- The required independence of the Safety Monitor is not yet defined.
- The detailed implementation of the independent torque-inhibit path is not specified.
- The load-holding / brake concept requires further definition and sizing.
- Diagnostic thresholds and reaction-time budgets are not yet derived.
- Sensor independence and diversity require further analysis.
- Quantitative diagnostic coverage and reliability targets are not yet calculated.
- Common-cause and dependent failures between control and safety paths require separate analysis.
Detailed common-cause and dependent-failure analysis remains future work.

These open points are kept explicit so that the architecture is not presented as more mature than it currently is.
