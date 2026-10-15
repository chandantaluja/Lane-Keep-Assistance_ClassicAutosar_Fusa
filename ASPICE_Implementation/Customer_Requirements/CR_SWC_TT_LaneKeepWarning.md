## 2. Lane Keep Assistance Warning Telltale SWC Requirements

### 2.1 Functional Requirements (Telltale State Machine)

* **`CR_LKA_TT_001` - Power-On Bulb Check:**  
  Upon vehicle ignition ON (System Check), the Telltale SWC shall illuminate the Amber/Yellow LKA warning light for $3 \text{ seconds}$ for bulb verification, then update according to current system status.

* **`CR_LKA_TT_002` - Telltale Visual Indicator States:**  
  The Telltale SWC shall set the visual display state (`Tx_LKA_TelltaleState`) according to the following logic matrix:

| LKA System Status | Visual Output State | Color / Behavior |
| :--- | :--- | :--- |
| **OFF / Standby** | `TELLTALE_OFF` | Icon OFF |
| **ON (Available / Active Tracking)** | `TELLTALE_WHITE` or `TELLTALE_GREEN` | Solid Green (Active) / White (Ready) |
| **Intervention In Progress** | `TELLTALE_FLASHING_GREEN / AMBER` | Flashing Green or Amber |
| **Hands-Off Warning (Driver Inattentive)** | `TELLTALE_AMBER_FLASHING` | Flashing Amber with Audio Request |
| **System Fault / Degraded** | `TELLTALE_AMBER_SOLID` | Solid Amber |

* **`CR_LKA_TL_003` - Audio Alert Synchronization:**  
  During an unintentional lane departure or persistent Hands-Off Warning ($> 15 \text{ seconds}$ hands off wheel), the Telltale SWC shall request an audible chime (`Tx_AudioChimeReq = ENABLED`) via the Instrument Cluster / HMI ECU.