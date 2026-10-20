# SYS _ Core Calculations & Torque Requests

This document lists the system requirements for the module Core Calculations & Torque Requests.


## Revision Table

|        S.No    | Author       | Desciption   | Date         |  
| :------------- |:-------------|:-------------|:-------------|
| 0.1            | Chandan      | Initial Draft|              |
| 0.2            |      |              |              |
| 0.3            |      |              |              |


## Requirements

| Customer Req Id | SYS_Req_Id | Text | Verification Criteria | Verification Method | Linked Test |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `CR_LKA_003` | `SYS_REQ_LKA_CALC_001` | **Departure Trajectory Prediction:** When in **ACTIVE** mode, the system shall compute Time-to-Line-Crossing (TLC) using `Rx_LaneOffset`, vehicle yaw rate, and `Rx_VehicleSpeed`. | Provide known vehicle trajectory inputs and verify calculated TLC matches numerical baseline within $\pm 0.01\text{ s}$. | SW Unit Test / SIL Test | |
| `CR_LKA_003` | `SYS_REQ_LKA_CALC_002` | **Target Corrective Torque Calculation:** When predicted TLC falls below the intervention threshold ($\le 0.8\text{ s}$) without an active turn indicator, the system shall calculate target corrective steering torque (`Tx_LKA_TargetTorque`). | Simulate lane deviation with $\text{TLC} = 0.7\text{ s}$; verify non-zero corrective torque output calculation. | SIL Test | |
| `CR_LKA_003` | `SYS_REQ_LKA_CALC_003` | **Torque Request Output Signaling:** The system shall transmit `Tx_LKA_TargetTorque` along with request status `Tx_LKA_ReqStatus = ACTIVE` over the AUTOSAR RTE Sender-Receiver interface to the Electric Power Steering (EPS) ECU. | Verify RTE output buffer reflects `Tx_LKA_ReqStatus = 1` (`ACTIVE`) and corresponding torque payload during intervention. | HIL Bus Monitoring Test | |

---