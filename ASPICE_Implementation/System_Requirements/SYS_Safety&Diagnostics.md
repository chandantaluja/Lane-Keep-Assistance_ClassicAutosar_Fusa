# SYS _ Safety & Diagnostics

This document lists the system requirements for the module Non-Volatile Memory (NvM) Management.


## Revision Table

|        S.No    | Author       | Desciption   | Date         |  
| :------------- |:-------------|:-------------|:-------------|
| 0.1            | Chandan      | Initial Draft|              |
| 0.2            |      |              |              |
| 0.3            |      |              |              |


## Requirements

| Customer Req Id | SYS_Req_Id | Text | Verification Criteria | Verification Method | Linked Test |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `CR_LKA_005` | `SYS_REQ_LKA_SAFE_001` | **Maximum Torque Saturation Limit:** The computed output `Tx_LKA_TargetTorque` shall be hard-saturated to a maximum absolute limit of $|3.0\text{ Nm}|$ in the application layer prior to RTE output transmission. | Request a calculated torque of $4.5\text{ Nm}$; verify output signal is hard-capped at exactly $3.0\text{ Nm}$. | SW Unit Test / SIL Test | |
| `CR_LKA_005` | `SYS_REQ_LKA_SAFE_002` | **Torque Gradient / Rate Limiting:** The rate of change of `Tx_LKA_TargetTorque` shall not exceed $5.0\text{ Nm/s}$ to prevent abrupt steering disturbances. | Apply step input target torque change from $0\text{ Nm}$ to $3.0\text{ Nm}$; verify signal rate slope $\le 5.0\text{ Nm/s}$. | SIL Test | |
| `CR_LKA_006` | `SYS_REQ_LKA_SAFE_003` | **Fault State Transition:** Upon confirmation of an input signal timeout or internal integrity failure, the state machine shall immediately transition to **FAULT** mode (`Tx_LKA_ReqStatus = FAULT`). | Inject signal timeout fault; verify state machine sets `Tx_LKA_ReqStatus = FAULT`. | SIL / HIL Fault Injection Test | |
| `CR_LKA_006` | `SYS_REQ_LKA_SAFE_004` | **Controlled Torque Ramp-Down Protocol:** Upon entering **FAULT** state, `Tx_LKA_TargetTorque` shall smoothly ramp down to $0.0\text{ Nm}$ at a uniform rate over a duration of $200\text{ ms}$. | Trigger fault during $3.0\text{ Nm}$ intervention; confirm torque reaches $0.0\text{ Nm}$ linearly over $200\text{ ms} \pm 10\text{ ms}$. | HIL Oscilloscope / Bus Log Analysis | |
| `CR_LKA_006` | `SYS_REQ_LKA_SAFE_005` | **Fault Tolerant Time Interval (FTTI) Budget:** The total fault handling sequence (detection to completion of torque ramp-down) shall complete within an FTTI budget of $300\text{ ms}$ ($100\text{ ms detection} + 200\text{ ms ramp-down}$). | Measure time elapsed from CAN message frame drop to $0.0\text{ Nm}$ torque verification; confirm total time $\le 300\text{ ms}$. | SIL / HIL Timing Analysis | |
| `CR_LKA_006` | `SYS_REQ_LKA_SAFE_006` | **Diagnostic Trouble Code (DTC) Event Reporting:** Upon signal timeout fault confirmation, the system shall report a Diagnostic Event (e.g., `LKA_E_SIGNAL_TIMEOUT`) to the AUTOSAR **Dem** (Diagnostic Event Manager) service port. | Trigger timeout fault; verify `Dem_SetEventStatus` API is called with status `DEM_EVENT_STATUS_FAILED` for event `LKA_E_SIGNAL_TIMEOUT`. | SIL / HIL Diagnostic Test | |