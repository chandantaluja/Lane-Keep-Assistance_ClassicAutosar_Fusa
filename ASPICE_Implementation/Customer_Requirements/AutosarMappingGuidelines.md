# ⚙️ AUTOSAR Implementation & Mapping Guidelines

When mapping these requirements into an AUTOSAR Classic Platform tool (e.g., Vector DaVinci Developer or ETAS ISOLAR-A):

• **SWC Types:**  
  • `SWC_LKA`: Application Software Component (contains core control algorithms and state machine).
  • `SWC_TellTaleManagement_LKA`: Sensor/Actuator SWC or Display Application SWC depending on ECU architecture.

• **Runnable Entities:**  
  • `BSW_Task_10ms`: Periodic runnable triggered by a 10ms OS Timer Event for core torque computation.
  • `Telltale_Update_50ms`: Periodic runnable triggered by a 50ms OS Timer Event for visual status update.

• **BSW Services Used:**  
  • **Dem Service Port:** For reporting fault events (e.g., sensor lost).
  • **NvM Service Port:** For persistent storage of driver ON/OFF preferenc