# Telltale SWC Interfaces & Signals (AUTOSAR Port Architecture)

| Port Name | Direction | Interface Type | Signal Name | Data Type | Values / Description |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `RPort_LKASystemState` | Input | Sender-Receiver | `Rx_LKA_SysState` | `uint8` | `OFF`, `READY`, `ACTIVE`, `WARNING`, `FAULT` |
| `RPort_HandsOnStatus` | Input | Sender-Receiver | `Rx_HandsOnDetected` | `boolean` | `TRUE` (Hands on), `FALSE` (Hands off) |
| `PPort_HMITelltale` | Output | Sender-Receiver | `Tx_LKA_TelltaleState` | `uint8` | `0=OFF`, `1=WHITE`, `2=GREEN`, `3=AMBER`, `4=FLASHING` |
| `PPort_AudioWarning` | Output | Client-Server / S-R | `Tx_AudioChimeReq` | `boolean` | `TRUE` = Play Chime, `FALSE` = Silence |

---
