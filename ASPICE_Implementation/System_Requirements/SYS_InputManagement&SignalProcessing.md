# SYS _ Input Management & Signal Processing

This document lists the system requirements for the module SYS_InputManagement & Signal Processing.


## Revision Table

|        S.No    | Author       | Desciption   | Date         |  
| :------------- |:-------------|:-------------|:-------------|
| 0.1            | Chandan      | Initial Draft|              |
| 0.2            |      |              |              |
| 0.3            |      |              |              |


## Requirements


| Customer Req Id | SYS_Req_Id | Text | Verification Criteria | Verification Method | Linked Test |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `CR_LKA_001` | `SYS_REQ_LKA_IN_001` | **HMI Switch Input Acquisition:** The system shall sample the driver switch status signal (`Rx_LKA_DriverSwitchStatus`) at a periodic task rate of $10\text{ ms}$. | Measure task scheduling interval on RTE; verify `Rx_LKA_DriverSwitchStatus` signal read cycle occurs every $10\text{ ms} \pm 1\text{ ms}$. | SIL / HIL Test | |
| `CR_LKA_001` | `SYS_REQ_LKA_IN_002` | **Switch Signal Debouncing:** The system shall perform signal debouncing on `Rx_LKA_DriverSwitchStatus` for a minimum duration of $50\text{ ms}$ before updating the debounced switch state. | Inject switch state pulse lasting $< 50\text{ ms}$ and verify debounced state does not update; inject pulse $\ge 50\text{ ms}$ and verify state updates successfully. | HIL Test / SW Unit Test | |
| `CR_LKA_006` | `SYS_REQ_LKA_IN_003` | **Signal Timeout Supervision:** The system shall continuously monitor reception intervals of critical inputs (`Rx_VehicleSpeed`, `Rx_LaneOffset`, `Rx_LaneConfidence`) and declare a signal missing event if any signal is delayed by $> 100\text{ ms}$. | Stop transmitting target CAN/RTE message; verify signal missing fault flag is set at $100\text{ ms} \pm 5\text{ ms}$. | SIL / HIL Fault Injection Test | |

---
