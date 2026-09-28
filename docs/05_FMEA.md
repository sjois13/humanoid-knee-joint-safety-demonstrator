# 5. FMEA

> Editable source: [`../source/05_FMEA.xlsx`](../source/05_FMEA.xlsx)

| ID | Item | Failure mode | Effect on joint/system | Detection | Reaction | Related FSR |
| --- | --- | --- | --- | --- | --- | --- |
| FMEA-01 | Joint Encoder | Plausible incorrect or frozen joint position | Controller has an incorrect view of knee position and may generate inappropriate corrective torque | Motor-vs-joint plausibility, rate/change monitoring | Restrict operation; degraded mode or controlled stop depending on remaining sensing | FSR-01, FSR-06, FSR-10 |
| FMEA-02 | Motor Encoder | Incorrect rotor position | Motor control/FOC may generate torque different from request | Motor-vs-joint plausibility, current and motion monitoring | Controlled reduction if reliable control remains; otherwise torque disable | FSR-06, FSR-10 |
| FMEA-03 | Current Sensing | Incorrect or unavailable current feedback | Motor torque may be higher or lower than intended because current regulation uses incorrect feedback | Commanded-vs-measured current, phase plausibility | Torque limitation; stop/disable if reliable current control is lost | FSR-04, FSR-05, FSR-10 |
| FMEA-04 | Communication Interface | Missing, stale or corrupted command | Joint may execute outdated or incorrect robot-level intent | Timeout, sequence counter, CRC/integrity monitoring | Reject invalid command; controlled stop / local hold | FSR-07, FSR-08 |
| FMEA-05 | Local Joint Controller | Incorrect or stuck torque/current request | Hazardous torque may be commanded even though sensors and hardware are healthy | Independent limits / Safety Monitor / watchdog | Independent reaction path; report to robot-level supervisor | FSR-02, FSR-09 |
| FMEA-06 | Motor Controller | Incorrect or unstable current regulation / PWM | Actual motor current and torque differ from requested values | Current deviation and joint-motion monitoring | Inhibit PWM / independent torque disable | FSR-04, FSR-05, FSR-09 |
| FMEA-07 | Gate Driver / Inverter | Unintended actuation or failure to disable | Torque may continue despite zero/disable command | Current persists after disable request, gate/power-stage diagnostics | Independent gate disable or power isolation | FSR-05, FSR-09 |
| FMEA-08 | Gearbox / Coupling | Transmission mismatch, slip or jam | Motor behaviour no longer corresponds to actual knee motion; support may be lost or mechanical load increased | Motor-vs-joint encoder plausibility, current vs motion | Limit torque; controlled stop / robot-level recovery | FSR-06 |
| FMEA-09 | Safety Monitor / Disable Path | Hazard not detected or torque inhibition unavailable | A control fault may propagate to HE-01 without mitigation | Self-test, watchdog, proof/diagnostic checks | Prevent unrestricted operation; robot-level controlled stop while normal control remains healthy | FSR-09 |
