# 5. FMEA and Fault Tree Analysis

This section examines how faults in the knee-joint architecture can contribute to HE-01: hazardous unintended knee motion while the joint is load-bearing.

The analysis focuses on failure paths that can create hazardous actuator behaviour, prevent its detection, or prevent the intended safety reaction.

## 5.1 Analysis Scope

The analysis covers failures across:

- sensing
- communication
- local control
- motor control and power electronics
- mechanical transmission
- safety monitoring
- fault-reaction functions

A failure mode is included when it can contribute directly to HE-01 or reduce the ability to detect or mitigate it.

## 5.2 FMEA Method

The focused FMEA uses the following reasoning chain:

**Failure Mode → Effect on Joint / System → Detection → Fault Reaction**

The analysis concentrates on how each failure can influence physical knee behaviour and which monitoring or reaction mechanism is intended to interrupt the fault propagation.

## 5.3 FMEA Observations

- A sensor fault can remain hazardous even when the reported value is still plausible.
- Shared sensing between control and monitoring can create common dependencies.
- Motor-side and joint-side encoders support useful plausibility checking across the transmission, but a mismatch does not necessarily identify which element has failed.
- Incorrect current feedback can cause actual torque to differ from the intended torque.
- A controller failure is more critical when the same controller is also required to execute the only available safety reaction.
- Power-stage faults can cause physical torque to differ from software intent.
- An unsafe command may be executed correctly, so command-versus-response monitoring alone is not sufficient.
- Mechanical transmission faults remain safety-relevant even when the electrical control path operates correctly.
- The Safety Monitor and independent torque-inhibit path must themselves be supervised for latent failures.

## 5.4 Fault Tree Analysis Approach

The FTA starts from HE-01 and works backward toward the main combinations of failures that can produce the hazardous event.

The tree is qualitative and intentionally simplified. It is used to show the main safety argument rather than calculate a top-event probability.

## 5.5 Fault Tree Structure

The top event, HE-01, is represented by:

**Hazardous actuator behaviour AND failure of the available safety mitigation**

Hazardous actuator behaviour may result from:

- incorrect command or control output
- motor-control or inverter fault
- incorrect encoder or current feedback

Safety mitigation failure may result from:

- failure of the Safety Monitor to detect the unsafe condition
- failure of the independent torque-inhibit path to stop hazardous torque

![Figure 5 — Simplified Fault Tree Analysis](../figures/05_fault_tree.png)

## 5.6 Dependent and Common-Cause Failures

The top-level AND gate represents the intended safety concept, but it does not by itself prove independence between the control and safety paths.

A common fault such as shared power, clock, processor resources or communication infrastructure could affect both the initiating path and the safety mechanism.

These dependent and common-cause failures are not fully developed in the simplified tree and require separate analysis as the architecture becomes more detailed.

## 5.7 FTA Logic and Interpretation

The initiating faults on the left are connected by an **OR** gate because any one may produce hazardous actuator behaviour.

The mitigation failures on the right are also connected by an **OR** gate.

The top-level **AND** gate shows the intended safety argument: an actuator fault should not lead to HE-01 if the safety mechanisms detect and control it successfully.

Communication and transmission faults are covered in the FMEA but are not shown as separate basic events in this simplified fault tree.

## 5.8 Link to Safety Requirements

The FMEA and FTA support the requirements derived in Section 4:

- FSR-01 / FSR-02 address joint-velocity deviation and the required reaction.
- FSR-04 / FSR-05 address motor-current deviation and torque-generation faults.
- FSR-06 addresses inconsistency between motor-side and joint-side motion.
- FSR-07 / FSR-08 address invalid or unavailable robot-level commands.
- FSR-09 provides a safety-reaction path that does not rely solely on normal control.
- FSR-10 addresses invalid or unavailable safety-relevant sensing.
- FSR-11 addresses unsafe actuator behaviour that may still be consistent with the commanded value.
- FSR-12 addresses loss or unavailability of the Safety Monitor or independent torque-inhibit path.

## 5.9 Limitations

This is a qualitative system-level analysis.

It does not yet include:

- component failure-rate or FIT data
- quantitative diagnostic coverage
- safe / dangerous failure classification
- achieved PL calculation
- quantitative top-event probability
- detailed common-cause failure analysis
- complete latent-fault analysis
- detailed failure analysis of the load-holding mechanism
