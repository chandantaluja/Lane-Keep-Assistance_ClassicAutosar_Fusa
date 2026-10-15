# Lane Keep Assistance (LKA) SWC Requirements

## 1. Lane Keep Assistance (LKA) SWC Requirements

### 1.1 Functional Requirements (LKA Core Logic)

* **`CR_LKA_001` - System Enable/Disable State:**
  The LKA SWC shall allow the driver to toggle the system ON/OFF via a human-machine interface (HMI) input signal (`Rx_LKA_DriverSwitchStatus`). The default state at power-on (Ignition ON) shall be OFF (or last persistent memory state).

* **`CR_LKA_002` - Activation Conditions:**
  The LKA SWC shall transition to the **ACTIVE** state only when all of the following conditions are simultaneously met:
  1. System is ON (`Rx_LKA_DriverSwitchStatus == ENABLED`).
  2. Vehicle speed is within operational boundaries ($60 \text{ km/h} \le \text{VehicleSpeed} \le 180 \text{ km/h}$).
  3. Lane confidence score from Vision Sensor is above threshold (`Rx_LaneConfidence >= 75%`).
  4. Driver turn indicator is **OFF** (`Rx_TurnSignalStatus == OFF`).
  5. Driver override/hands-on detection is non-intervention level (`Rx_DriverTorque < Threshold_Override`).

* **`CR_LKA_003` - Lane Departure Intervention:**
  When the LKA SWC detects that the vehicle is crossing or about to cross a detected lane boundary without an active turn indicator:
  1. It shall calculate the required corrective steering torque (`Tx_LKA_TargetTorque`).
  2. It shall send a torque intervention request (`Tx_LKA_TorqueReqStatus = ACTIVE`) to the Electric Power Steering (EPS) ECU over AUTOSAR RTE.

* **`CR_LKA_004` - Driver Override / Deactivation:**
  The LKA SWC shall immediately suppress target torque output (`Tx_LKA_TargetTorque = 0 Nm`) and transition to **SUPPRESSED** state if:
  * Driver applies manual steering torque exceeding the override limit ($> 2.5 \text{ Nm}$).
  * Driver turns on either directional turn indicator.
  * Driver applies braking or sudden acceleration override.

### 1.2 Interfaces & Signals (AUTOSAR Port Architecture)

| Port Name | Direction | Interface Type | Signal Name | Data Type | Range / Description |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `RPort_VehicleSpeed` | Input | Sender-Receiver | `Rx_VehicleSpeed` | `float32` | 0.0 to 250.0 km/h |
| `RPort_LaneData` | Input | Sender-Receiver | `Rx_LaneOffset`, `Rx_LaneConfidence` | `float32`, `uint8` | Distance to lane, Confidence (0-100%) |
| `RPort_DriverInputs` | Input | Sender-Receiver | `Rx_DriverTorque`, `Rx_TurnSignal` | `float32`, `uint8` | Measured Steering Torque (Nm), Indicator State |
| `PPort_LKATorqueReq` | Output | Sender-Receiver | `Tx_LKA_TargetTorque`, `Tx_LKA_ReqStatus` | `float32`, `uint8` | Calculated Correction Torque (Nm), State |

---

## 2. Safety & Diagnostic Requirements (ISO 26262 ASIL B/C)

* **`CR_LKA_005` - Maximum Torque Limitation:**
  The maximum corrective torque commanded by LKA shall not exceed $3.0 \text{ Nm}$ to prevent un-commanded vehicle motion or loss of control by the driver.

* **`CR_LKA_006` - Signal Timeout & Fault Handling:**
  If any critical input signal (e.g., `Rx_VehicleSpeed`, `Rx_LaneData`) fails or experiences a timeout ($> 100 \text{ ms}$), the LKA SWC shall:
  1. Immediately set `Tx_LKA_ReqStatus = FAULT`.
  2. Ramp down `Tx_LKA_TargetTorque` to $0 \text{ Nm}$ within $200 \text{ ms}$.
  3. Report a Diagnostic Trouble Code (DTC) to the Dem (Diagnostic Event Manager) module via AUTOSAR Service Component interfaces.