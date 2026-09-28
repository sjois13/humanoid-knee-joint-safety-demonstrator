# 5. FMEA and Fault Tree Analysis

This section examines how faults in the knee-joint architecture can contribute to HE-01: hazardous unintended knee motion while the joint is load-bearing. The analysis is deliberately focused on the failure paths that matter to the demonstrator rather than attempting a complete production FMEA or FMEDA.

## 5.1 Analysis Scope

The analysis covers failures across sensing, communication, control, power electronics, mechanical transmission and the safety-reaction path. The aim is to understand how a local failure can propagate to hazardous joint behaviour and where detection or mitigation can interrupt that propagation.

A failure mode is retained in the portfolio analysis only when it contributes directly to HE-01, affects the ability to detect HE-01, or affects the ability to mitigate it. This keeps the analysis aligned with the project scope.

## 5.2 FMEA Method

The focused FMEA uses the following reasoning chain:

**Failure Mode → Local Effect → System Effect → Detection → Fault Reaction**

The distinction between local and system effect is important. The local effect describes what changes around the failed item. The system effect describes how that change can influence the knee joint or contribute to the hazardous event.

## 5.3 FMEA Observations

- Sensor faults can be hazardous even when the sensor output remains plausible.
- Incorrect current feedback can result in either excessive torque or unintended torque reduction, depending on the failure direction.
- Motor-side and joint-side encoders provide different information and therefore support useful cross-checking across the gearbox.
- A local controller failure is more critical when the same controller is also required to execute the only available safety reaction.
- Power-stage faults can cause physical torque to differ from software intent, so monitoring only commands is insufficient.
- Mechanical transmission faults remain safety-relevant even when all electronics operate correctly.
- The safety monitor and independent torque-disable path must themselves be considered in the failure analysis.

## 5.4 Fault Tree Analysis Approach

The FTA complements the FMEA by starting from the hazardous system behaviour and reasoning backward toward possible causes. The tree is intentionally small and is used to show the main causal structure rather than quantify top-event probability.

## 5.5 Fault Tree Structure

The top event, HE-01, requires both:

**Hazardous actuator behaviour AND failure of the available safety mitigation.**

Hazardous actuator behaviour may result from incorrect command/control output, motor-control or inverter faults, or incorrect feedback.

Safety mitigation failure may result from failure of the Safety Monitor or the independent torque-disable path.

![Figure 5 — Simplified Fault Tree Analysis](../figures/05_fault_tree.png)

## 5.7 FTA Logic and Interpretation

The initiating faults on the left are connected by an OR gate because any one may cause hazardous actuator behaviour.

The mitigation failures on the right are also connected by an OR gate.

The top-level AND gate shows that HE-01 occurs when hazardous actuator behaviour is present and the intended mitigation does not successfully control it.

## 5.8 Link to Safety Requirements

The FMEA and FTA support the requirements derived in Section 4. The main relationships are:

- FSR-01 / FSR-02 address hazardous joint-velocity deviation and the required reaction.
- FSR-04 / FSR-05 address unintended motor-current behaviour and torque-generation faults.
- FSR-06 addresses inconsistency between motor-side and joint-side motion.
- FSR-07 / FSR-08 address invalid or unavailable robot-level commands.
- FSR-09 addresses the possibility that the normal control path itself is faulty.
- FSR-10 addresses invalid or unavailable safety-relevant sensing.

## 5.9 Limitations

This is a qualitative system-level analysis. It does not include component failure rates, FIT data, diagnostic coverage calculations, quantitative probability evaluation, or a complete dependent-failure analysis. These would require a more detailed hardware design and reliability data.

The purpose of this analysis is to demonstrate the link between architecture, failure propagation, safety mechanisms and requirements for the selected hazardous event.

## 5.10 Related Project Artifacts

- `05_FMEA.xlsx` — full focused FMEA working table
- `05_fault_tree.drawio` — editable fault-tree source
- `05_fault_tree.png` or `.svg` — exported fault-tree figure for GitHub and the final report
