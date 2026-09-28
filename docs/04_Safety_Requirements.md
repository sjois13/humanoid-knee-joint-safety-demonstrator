# 4. Safety Requirements

## 4.1 Purpose

The safety requirements are derived from the hazardous event HE-01 and the associated safety goal SG-01.

The requirements below define the main detection, monitoring and fault-reaction functions needed to support this safety goal.

At this stage, exact numerical thresholds and reaction times are not assigned. These would need to be derived later from actuator dynamics, robot load cases and allowable fault-response time.

## 4.2 Requirement Derivation Approach

The requirements were derived by looking at the main ways HE-01 could develop through the architecture.

The intent is not only to detect individual component faults, but also to monitor whether the physical actuator behaviour remains consistent with the commanded behaviour.

## 4.3 Functional Safety Requirements

### FSR-01 — Joint Velocity Monitoring

The knee joint system shall monitor the deviation between commanded joint velocity and measured joint velocity and detect when the deviation exceeds a defined allowable limit.

**Rationale:** Unexpected joint velocity is one direct indication that the physical knee behaviour is no longer following the intended motion.

### FSR-02 — Reaction to Excessive Velocity Deviation

When the commanded-to-measured joint velocity deviation exceeds the defined limit for a defined duration, the knee joint system shall initiate a safety reaction to limit or interrupt hazardous torque generation.

**Rationale:** Detection alone is insufficient. A persistent motion deviation must result in a reaction before it develops into uncontrolled knee movement.

### FSR-03 — Fault Reporting

The knee joint system shall report detected safety-relevant faults to the robot-level safety supervisor.

**Rationale:** The local knee controller can detect actuator-level faults, but recovery may require whole-body actions

### FSR-04 — Motor Current Monitoring

The knee joint system shall monitor the deviation between commanded motor current and measured motor current and detect when the deviation exceeds a defined allowable limit.

**Rationale:** Motor current is directly related to torque generation. A significant commanded-versus-measured current deviation can indicate that the electrical actuation path is no longer behaving as requested.

### FSR-05 — Reaction to Excessive Current Deviation

When a safety-relevant motor-current deviation is detected, the knee joint system shall initiate a defined fault reaction to prevent continued hazardous torque generation.

**Rationale:** If actual current is no longer following the commanded current, normal torque control cannot automatically be assumed to remain trustworthy.

### FSR-06 — Transmission Plausibility Monitoring

The knee joint system shall monitor the relationship between motor-side motion and joint-side motion and detect when the measured relationship deviates from the expected transmission ratio beyond a defined allowable limit.

**Rationale:** Motor motion should have a predictable relationship with knee motion through the gearbox.

### FSR-07 — Communication Supervision

The knee joint system shall detect missing, delayed, corrupted or stale safety-relevant commands received from the whole-body controller.

**Rationale:** The local joint system cannot assume that every received command is fresh and valid.

### FSR-08 — Reaction to Communication Fault

The knee joint system shall initiate a defined safe reaction when safety-relevant communication from the whole-body controller is missing, delayed, corrupted or stale.

**Rationale:** Once the local joint can no longer rely on new robot-level commands, it should not continue unrestricted normal operation.

### FSR-09 — Independent Safety Reaction Path

The knee joint system shall provide a safety reaction path that is sufficiently independent of the normal control path such that a single fault in the normal control path does not prevent mitigation of hazardous joint torque or motion.

**Rationale:** If the normal joint controller itself generates the hazardous torque request, relying only on the same controller to remove that torque creates a single-point dependency.

### FSR-10 — Sensor Fault Handling

The knee joint system shall, upon detection of an invalid or unavailable safety-relevant sensor signal, prevent unrestricted actuator operation and transition to a defined degraded mode or safe reaction based on the remaining valid sensing capability.

**Rationale:** A single sensor fault does not necessarily require immediate torque removal. The reaction depends on whether enough trustworthy information remains to continue limited control safely.

## 4.4 Notes and Open Points

The following items remain to be derived during more detailed design:

- joint-velocity deviation threshold
- current-deviation threshold
- persistence times before declaring a fault
- maximum diagnostic and reaction times
- exact communication timeout
- sensor accuracy and plausibility limits
- required independence of the Safety Monitor
- detailed degraded operating modes
- exact torque-disable implementation.

These values should not be selected arbitrarily. They need to come from actuator dynamics, expected operating conditions, system-level response time and the consequences of delayed fault mitigation.
