# Lane Keep Assistance (LKA) & ADAS Requirements Repository

This repository contains the system, software, and customer requirements (CRs) for the Lane Keep Assistance (LKA) Software Component (SWC), Telltale SWC, and associated AUTOSAR interfaces, aligned with **ISO 26262 (Functional Safety)** guidelines.

---

## 📁 Folder Structure

```text
├── CRS_LaneKeepAssistance.md       # Core functional, safety, and interface requirements for LKA SWC
├── CRS_SWC_TT_LaneKeepWarning.md  # Requirements for LKA Warning Telltale
├── AutosarMappingGuidelines.md  # Requirements for 
├── AutosarPortArchitecture.md  # Requirements for 
└── README.md                 # Repository documentation and guide   
```
## Contributing & Updates
When adding or modifying Customer Requirements (CRs):  
 • Maintain unique ID naming conventions (CR_LKA_XXX).  
 • Ensure safety-relevant constraints (ASIL classifications, torque thresholds, timeout limits) are explicitly documented.  
 • Keep port names and data types synchronized with the AUTOSAR component interface description.  
