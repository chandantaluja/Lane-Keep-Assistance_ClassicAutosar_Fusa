# SYS _ State Machine & Activation Control

This document lists the system requirements for the module State Machine & Activation Control.


## Revision Table

|        S.No    | Author       | Desciption   | Date         |  
| :------------- |:-------------|:-------------|:-------------|
| 0.1            | Chandan      | Initial Draft|              |
| 0.2            |      |              |              |
| 0.3            |      |              |              |


## Requirements

| Customer Req Id | SYS_Req_Id | Text | Verification Criteria | Verification Method | Linked Test |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `CR_LKA_001` | `SYS_REQ_LKA_STATE_001` | **Default Power-On Initialization:** Upon Ignition ON event, the LKA system state machine shall initialize to the **OFF** state prior to reading persistent storage. | Monitor initial SWC state variable immediately post-boot (before NvM callback execution); confirm state equals `STATE_OFF`. | SIL / HIL Power Cycle Test | |
| `CR_LKA_002` | `SYS_REQ_LKA_STATE_002` | **Operational Speed Boundary Verification:** The system shall permit transition to **STANDBY** mode only when vehicle speed satisfies $60.0\text{ km/h} \le \text{Rx\_VehicleSpeed} \le 180.0\text{ km/h}$. | Sweep `Rx_VehicleSpeed` from $0$ to $200\text{ km/h}$; verify transition to **STANDBY** occurs strictly between $60.0\text{ km/h}$ and $180.0\text{ km/h}$. | HIL Test | |
| `CR_LKA_002` | `SYS_REQ_LKA_STATE_003` | **Perception Confidence Qualification:** The system shall require vision sensor lane confidence `Rx_LaneConfidence \ge 75\%` as a prerequisite for entering **ACTIVE** tracking mode. | Inject `Rx_LaneConfidence = 74\%` and verify transition to **ACTIVE** is blocked; inject $75\%$ and confirm transition is permitted. | SIL Test | |
| `CR_LKA_002` | `SYS_REQ_LKA_STATE_004` | **Activation Entry Matrix:** The system state machine shall transition from **STANDBY** to **ACTIVE** mode if and only if valid operational speed, lane confidence ($\ge 75\%$), indicator OFF status, and driver manual torque below threshold ($< 2.5\text{ Nm}$) are simultaneously satisfied for at least $20\text{ ms}$. | Set all conditions valid for $15\text{ ms}$ (verify no transition); hold for $\ge 20\text{ ms}$ and confirm state machine enters `STATE_ACTIVE`. | SIL / HIL Test | |

---