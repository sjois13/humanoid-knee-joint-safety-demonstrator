# 3. Hazard Analysis

## 3.1 Purpose

The objective of this analysis is to identify the hazardous system behaviour that can result from unintended knee-joint actuation.

The selected safety concern is:

Unintended or excessive knee torque / motion while the joint is load-bearing.

## 3.2 Operating Scenario

The reference operating scenario is:

The humanoid robot is walking slowly near a person while carrying a load.

During this operation:

- the knee is load-bearing
- the robot is moving relative to the person
- the carried load adds additional mechanical energy and consequence
- knee torque is required for both motion and posture control

## 3.3 Malfunctioning Behaviour

The malfunctioning behaviour considered is:

The knee produces unintended or excessive torque or moves at a velocity inconsistent with the commanded joint behaviour.

## 3.4 Hazardous Event — HE-01

HE-01 — Unintended or excessive knee motion occurs while the humanoid is load-bearing and operating close to a person.

The unexpected knee behaviour may cause loss of controlled walking or posture.

Possible consequences include:

- collision with the nearby person
- loss of balance
- robot fall
- destabilisation or dropping of the carried load
- impact of the robot or payload with surrounding equipment

The severity of the outcome depends on robot mass, payload, joint velocity and the operating environment. These parameters are not quantified in this demonstrator.

## 3.5 Possible Initiating Causes

The hazardous event can result from failures in several domains.

These causes are explored further in the FMEA and FTA rather than fully analysed here.

## 3.6 Hazard Analysis Summary

| Field | Description |
| --- | --- |
| Operating situation | Humanoid walking slowly near a person while carrying a load. Knee is load-bearing. |
| Malfunctioning behaviour | Knee moves faster than commanded or produces unintended / excessive torque. |
| Potential consequence | Collision, fall, dropped or destabilised load, injury to a person, or damage to equipment / robot. |
| Hazardous event | Unintended knee motion occurs while load-bearing and close to a person, resulting in loss of controlled robot motion or posture. |
| Possible causes | Communication fault, controller fault, encoder fault, current-sensing fault, motor-control fault, inverter fault, transmission fault, safety-mechanism failure. |
| Required safety measures | Monitor commanded versus actual behaviour, cross-check sensing, supervise communication, limit torque / velocity, and provide a sufficiently independent fault-reaction path. |
| Safe reaction consideration | Immediate torque-off may itself create a hazard because the knee is load-bearing. |

## 3.7 Safety Goal — SG-01

From HE-01, the following high-level safety goal is defined:

**SG-01 — The knee joint system shall prevent or limit unintended or excessive joint torque during load-bearing operation such that hazardous uncontrolled knee motion is avoided.**

## 3.8 Safe-State Consideration

A key outcome of the hazard analysis is that torque-off cannot be assumed to be the universal safe state.

For a load-bearing knee:

`Immediate torque removal → loss of joint support → possible collapse`

Depending on the detected fault and the remaining trustworthy functions, a safer reaction may instead be:

- torque limitation
- degraded operation
- controlled stop
- brake / hold
- independent torque disable
- robot-level recovery.

The final reaction therefore depends on which part of the control and actuation chain remains trustworthy after the fault.

## 3.9 Scope Limitation

This analysis does not assign a formal risk class or integrity level.

A production hazard analysis would require additional information such as:

- robot mass and payload
- joint torque and velocity limits
- human exposure conditions
- operating modes
- workspace assumptions
- applicable machinery / robotics standards
- severity and probability criteria.

For this demonstrator, the hazard analysis is used primarily to derive the safety goal and safety requirements and to support the subsequent FMEA, FTA and verification concept.
