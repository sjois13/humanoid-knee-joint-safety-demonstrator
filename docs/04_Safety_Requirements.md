# 4. Safety Requirements

## 4.1 Purpose

The safety requirements are derived from hazardous event HE-01 and safety goal SG-01.

They define the main monitoring, fault-detection and fault-reaction functions required at the knee-joint level.

Numerical limits and reaction times are not fixed yet. They will need to be derived from actuator dynamics, load cases, the permitted operating envelope and the available time to control the hazard.

## 4.2 Requirement Derivation Approach

The requirements were derived from the main ways HE-01 can develop through the command, sensing, control and actuation paths.

The concept therefore uses both:

- command-versus-response monitoring
- command-independent safety limits

This distinction is important because an unsafe command can be executed correctly without creating a command-to-response deviation.

## 4.3 Functional Safety Requirements

### FSR-01 — Joint Velocity Deviation Monitoring

The knee joint system shall monitor the deviation between commanded joint velocity and measured joint velocity.

A fault shall be detected when the deviation exceeds `V_DEV_MAX` for longer than `T_VEL_PERSIST`.

**Rationale:** A significant deviation indicates that the physical knee response is no longer following the requested motion.

### FSR-02 — Reaction to Excessive Velocity Deviation

Following detection of an excessive joint-velocity deviation, the knee joint system shall initiate the applicable safety reaction within `T_REACT_MAX`.

The reaction shall prevent continued unrestricted hazardous joint motion.

**Rationale:** Detection must be followed by a sufficiently fast reaction to prevent the deviation developing into hazardous motion.

### FSR-03 — Fault Reporting

The knee joint system shall report detected safety-relevant faults and degraded operating states to the Robot Safety Supervisor.

**Rationale:** Local detection may require coordinated whole-body action such as stopping, posture recovery or higher-level power isolation.

### FSR-04 — Motor Current Monitoring

The knee joint system shall monitor the deviation between commanded motor current and measured motor current.

A fault shall be detected when the deviation exceeds `I_DEV_MAX` for longer than `T_CURR_PERSIST`.

**Rationale:** Motor current provides information about the electrical torque-generation path. A significant deviation can indicate loss of normal torque control.

### FSR-05 — Reaction to Excessive Current Deviation

Following detection of a safety-relevant motor-current deviation, the knee joint system shall initiate the applicable safety reaction within `T_REACT_MAX`.

The reaction shall prevent continued unrestricted hazardous torque generation.

**Rationale:** If measured current no longer follows the requested current, normal torque control cannot automatically be assumed to remain trustworthy.

### FSR-06 — Transmission Plausibility Monitoring

The knee joint system shall monitor the relationship between motor-side motion and joint-side motion.

A fault shall be detected when the measured behaviour deviates from the expected transmission relationship beyond the defined plausibility limits.

**Rationale:** Motor-side and joint-side motion should remain physically consistent through the transmission. A mismatch may indicate a sensor, gearbox, coupling or configuration fault.

### FSR-07 — Communication Supervision

The knee joint system shall detect missing, delayed, corrupted or stale safety-relevant commands received from the Whole-Body Motion Controller.

**Rationale:** The local knee system cannot assume that every received command is valid and current.

### FSR-08 — Reaction to Communication Fault

Following detection of invalid or unavailable safety-relevant communication, the knee joint system shall prevent unrestricted continuation of normal operation and initiate the applicable degraded or safety reaction.

**Rationale:** Loss of trustworthy robot-level commands requires the local joint to move to a bounded operating condition.

### FSR-09 — Independent Safety Reaction Path

The knee joint system shall provide a hardware-capable safety reaction path that does not rely solely on the normal control software.

A single fault in the normal control path shall not prevent the safety reaction path from inhibiting hazardous torque generation.

**Rationale:** A fault in the normal controller must not also remove the only available means of mitigating the resulting hazardous actuation.

### FSR-10 — Sensor Fault Handling

When a safety-relevant sensor is detected as invalid or unavailable, the knee joint system shall prevent unrestricted actuator operation.

The subsequent reaction shall depend on the remaining trustworthy sensing and control capability.

**Rationale:** A single sensor fault does not always require immediate torque removal, but operation must remain bounded by the information that is still considered valid.

### FSR-11 — Independent Safety Envelope Monitoring

The knee joint system shall independently monitor whether measured joint motion and torque-related actuation remain within the permitted safety envelope.

The monitored limits shall include, where applicable:

- maximum permitted joint velocity
- permitted joint-position range
- maximum permitted torque or torque-producing current

Exceeding a safety-envelope limit shall initiate the applicable safety reaction.

**Rationale:** Command-versus-response monitoring cannot detect an unsafe command that is executed correctly.

### FSR-12 — Safety Mechanism Supervision

The knee joint system shall detect loss, unavailability or detected malfunction of the Safety Monitor or independent torque-inhibit path.

Unrestricted operation shall not be permitted while a required safety mechanism is unavailable.

**Rationale:** A latent failure in the safety path could otherwise remain undetected until another fault creates hazardous actuator behaviour.

## 4.4 Timing Constraint

The total safety reaction shall satisfy:

`T_DETECT + T_DECIDE + T_ACTUATE + T_PHYSICAL < T_HAZARD`

where:

- `T_DETECT` — fault-detection time
- `T_DECIDE` — safety logic processing time
- `T_ACTUATE` — time for the safety mechanism to act
- `T_PHYSICAL` — time for joint torque or motion to respond
- `T_HAZARD` — available time before the motion becomes hazardous

The numerical timing budget is still to be derived.

## 4.5 Parameters to Be Derived

| Parameter | Meaning |
| --- | --- |
| `V_DEV_MAX` | Maximum permitted commanded-to-measured velocity deviation |
| `I_DEV_MAX` | Maximum permitted commanded-to-measured current deviation |
| `T_VEL_PERSIST` | Persistence time for velocity-deviation detection |
| `T_CURR_PERSIST` | Persistence time for current-deviation detection |
| `V_SAFE_MAX` | Maximum permitted safety-monitored joint velocity |
| `Q_MIN / Q_MAX` | Permitted joint-position range |
| `TQ_SAFE_MAX` | Maximum permitted joint torque / torque-related actuation |
| `T_COMM_MAX` | Maximum permitted communication age / timeout |
| `T_REACT_MAX` | Maximum permitted time from fault detection to safety reaction |

These values must be derived from the actuator dynamics, mechanical load cases, operating scenario and hazard-response time rather than selected independently.
