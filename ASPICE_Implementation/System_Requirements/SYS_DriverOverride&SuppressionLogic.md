# SYS _ Driver Override & Suppression Logic

This document lists the system requirements for the module Driver Override & Suppression Logic.


## Revision Table

|        S.No    | Author       | Desciption   | Date         |  
| :------------- |:-------------|:-------------|:-------------|
| 0.1            | Chandan      | Initial Draft|              |
| 0.2            |      |              |              |
| 0.3            |      |              |              |


## Requirements

| Customer Req Id | SYS_Req_Id | Text | Verification Criteria | Verification Method | Linked Test |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `CR_LKA_004` | `SYS_REQ_LKA_OVR_001` | **Manual Steering Override:** If driver manual steering torque `Rx_DriverTorque` exceeds $2.5\text{ Nm}$ during an active intervention, the system shall immediately transition to **SUPPRESSED** state and command `Tx_LKA_TargetTorque = 0.0\text{ Nm}` within $10\text{ ms}$. | Inject `Rx_DriverTorque = 2.6\text{ Nm}` during intervention; verify torque output drops to $0.0\text{ Nm}$ in $\le 10\text{ ms}$. | HIL Test | |
| `CR_LKA_004` | `SYS_REQ_LKA_OVR_002` | **Directional Indicator Override:** If either directional indicator is activated (`Rx_TurnSignalStatus != OFF`) during an intervention, the system shall abort steering assist and transition to **SUPPRESSED** state within $10\text{ ms}$. | Toggle turn signal to `LEFT_ACTIVE` or `RIGHT_ACTIVE`; verify state changes to **SUPPRESSED** within $10\text{ ms}$. | SIL / HIL Test | |
| `CR_LKA_004` | `SYS_REQ_LKA_OVR_003` | **Longitudinal Override (Braking/Acceleration):** If driver active braking or aggressive acceleration override is detected, the system shall suppress torque assistance immediately. | Inject brake pedal switch `ACTIVE`; verify immediate suppression of steering assist request. | HIL Test | |
| `CR_LKA_004` | `SYS_REQ_LKA_OVR_004` | **Automatic Recovery to Standby:** Following an override event, the system shall remain in **SUPPRESSED** state until driver torque remains below $1.0\text{ Nm}$ for a minimum of $500\text{ ms}$, after which it shall re-evaluate **STANDBY** conditions. | Reduce driver torque to $0.8\text{ Nm}$ for $400\text{ ms}$ (confirm remaining in **SUPPRESSED**); maintain for $> 500\text{ ms}$ and confirm transition to **STANDBY**. | SIL Test | |

---