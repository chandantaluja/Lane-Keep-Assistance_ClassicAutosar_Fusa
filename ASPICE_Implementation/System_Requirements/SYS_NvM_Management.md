# SYS _ Non-Volatile Memory (NvM) Management

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
| `CR_LKA_001` | `SYS_REQ_LKA_NVM_001` | **Persistent Preference Restoration:** Upon boot completion, the system shall read the previously saved LKA driver preference state (ON/OFF) from Non-Volatile Memory via the AUTOSAR **NvM** block service interface. | Pre-load NvM block with `STATE_ON`; power-cycle ECU and verify system restores state to `ON`. | SIL / HIL NvM Mock Test | |
| `CR_LKA_001` | `SYS_REQ_LKA_NVM_002` | **Persistent Preference Storage:** The system shall issue an NvM write request to store the current debounced `Rx_LKA_DriverSwitchStatus` whenever a valid driver toggle event or Ignition OFF shutdown sequence occurs. | Toggle driver switch; verify `NvM_WriteBlock` API call is issued with updated state parameter. | HIL Test | |

---