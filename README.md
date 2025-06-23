# Activities Generator Project README

## Project Overview

Development of an **Activities Generator** that integrates with the existing **WBS Generator v3.5** to automatically create comprehensive activity schedules for electrical commissioning projects. The system will generate P6-compatible imports with activities, relationships, durations, and resource assignments.

## System Architecture

```
Equipment List → WBS Generator v3.5 → Activities Generator → P6 Import
```

### Integration Points
- **Input:** Equipment list with subsystem organization
- **WBS Integration:** Links to WBS codes 1000+ from WBS Generator
- **Output:** P6-compatible Excel with TASK, TASKPRED, TASKRSRC sheets
- **Scope:** Factory Acceptance Testing (FAT) and Site Acceptance Testing (SAT)

## Current Project Context

### Project Structure
- **2 Subsystems:** Switchroom (WBS 1010) + 33/11kV Substation (WBS 1247)  
- **Phases:** FAT and SAT within each subsystem
- **Equipment Coverage:** 305 WBS items with comprehensive equipment hierarchy

### Equipment Hierarchy Patterns
```
+UH (Protection Panels)
├── +UH | Protection Devices
│   └── -F (Protection relays)
├── +UH | Network Devices  
│   ├── -Y (Ethernet Switches)
│   └── -Y (GPS Network Clock)
└── +UH | Control Devices
    ├── -KF (Automation Controllers)
    └── -UC (Rectifiers)

+WA (HV/LV Switchboards)
├── +WA | Circuit Breakers
│   └── CB (Individual circuit breakers)
├── +WA | Current Transformers
│   └── CT (Individual CTs)
└── +WA | Voltage Transformers
    └── VT (Individual VTs)
```

## P6 Import Format

### Required Sheets
- **TASK:** Activities with task_code, status_code, wbs_id, wbs_name
- **TASKPRED:** Relationships with pred_task_id, task_id, pred_type, lag_hr_cnt  
- **TASKRSRC:** Resource assignments
- **RSRC:** Resource definitions
- **USERDATA:** System settings

### Key Integration Features
- **WBS Mapping:** Activities link to WBS codes 1000+
- **Unique Activity IDs:** Conflict-free task codes
- **Relationship Types:** FS (Finish-Start), SS (Start-Start), etc.
- **Resource Assignment:** Equipment-specific crew assignments

---

# FAT Activities Library

## Global Activities

### FAT | Preparations and Set-up
*Applied to all subsystems*

| Activity | Duration | Relationship |
|----------|----------|--------------|
| Deliniations in place | 4 hrs | Parallel |
| Signage in place | 4 hrs | Parallel |
| Tables and work area established | 2 hrs | Parallel |
| Temporary cabling installed | 4 hrs | Parallel |
| Drawing folders | 24 hrs | Parallel |
| SWMS Reviewed and finalised | 2 hrs | Parallel |

**Total Duration:** 40 hours

## Protection Equipment

### +UH (Protection Panels)
| Activity | Duration | Relationship |
|----------|----------|--------------|
| Energise equipment (One by One) - Verify supply circuits | 0.08 hrs | Sequential |
| Manually program IP addresses | 0.05 hrs | Sequential |
| Manually program IP Gateway | 0.05 hrs | Sequential |

**Total Duration:** 0.18 hours  
**Relationship:** Sequential → enables child equipment

### -F (Standard Protection Devices)
**Prerequisites (must complete before work activities):**
- Logic Diagrams (0 hrs milestone)
- Protection Settings Report (0 hrs milestone)  
- Setting Files (0 hrs milestone)

**Work Activities (parallel after prerequisites):**
| Activity | Duration |
|----------|----------|
| Input / Outputs (Operation only) | 1.5 hrs |
| Settings Download | 0.5 hrs |
| LED Config and Labels | 1 hr |
| Secondary Injections | 14.25 hrs |

**Total Duration:** 17.75 hours

### -F101 (33kV Switchboard Protection)
**Prerequisites to Secondary Injections:**
| Activity | Duration |
|----------|----------|
| Logic Diagrams | 0 hrs (milestone) |
| Protection Settings Report | 0 hrs (milestone) |
| Setting Files | 0 hrs (milestone) |
| Input / Outputs (Operation only) | 1.5 hrs |
| Settings Download | 0.5 hrs |
| LED Config and Labels | 1 hr |

**Final Activity:**
| Activity | Duration |
|----------|----------|
| Secondary Injections | 12.25 hrs |

**Total Duration:** 15.25 hours

### -F102 (BESS Feeder Protection)
**Prerequisites to Secondary Injections:**
| Activity | Duration |
|----------|----------|
| Input / Outputs (Operation only) | 1.5 hrs |
| Settings Download | 0.5 hrs |
| LED Config and Labels | 1 hr |

**Final Activity:**
| Activity | Duration |
|----------|----------|
| Secondary Injections | 12.75 hrs |

**Total Duration:** 15.75 hours

### -F (LV Switchboard Protection)
| Activity | Duration | Relationship |
|----------|----------|--------------|
| Input / Outputs (Operation only) | 1.5 hrs | Parallel |
| Settings Download | 30 mins | Parallel |
| LED Config and Labels | 1 hr | Parallel |

**Total Duration:** 3 hours

## Network Equipment

### -Y (Ethernet Switches - HV/LV)
**Sequential Activities (1-11):**
| Order | Activity | Duration |
|-------|----------|----------|
| 1 | Photo of OEM Product label attached | 5 mins |
| 2 | Label photo: "Model Number" visible (Y/N) | 5 mins |
| 3 | Label photo: "Serial Number" visible (Y/N) | 5 mins |
| 4 | Ensure installed correctly and free from damage | 5 mins |
| 5 | Before energizing check power supply polarity and magnitude | 5 mins |
| 6 | Remove power from device and confirm watchdog relay closes | 10 mins |
| 7 | Device configuration completed. Ensure no errors occure | 1.5 hrs |
| 8 | Confirm correct VLAN configuration | 1.5 hrs |
| 9 | Confirm Time Zone | 5 mins |
| 10 | Confirm DST | 5 mins |
| 11 | Drawings marked up - Bluelines Completed | 1 hr |

**Final Activities (12-14, after 1-11 complete):**
| Order | Activity | Duration |
|-------|----------|----------|
| 12 | Confirm all communications - HEALTHY (Y/N) | 1 hr |
| 13 | Confirm As-Left Firmware Version | 5 mins |
| 14 | As Left settings saved on RJE server | 30 mins |

**Total Duration:** ~6.73 hours  
**Relationship:** Sequential (1→2→...→14)

### -Y (GPS Clock)
*Same as regular -Y except activity 8:*
| Order | Activity | Duration |
|-------|----------|----------|
| 8 | Confirm correct communication protocols are enabled | 1.5 hrs |

**Total Duration:** ~6.73 hours  
**Relationship:** Sequential (1→2→...→14)

## Control Equipment

### -KF (Automation Controllers)
**Parallel Activities (can run in any order):**
| Activity | Duration |
|----------|----------|
| Photo of OEM Product label attached | 0.08 hrs |
| Label photo: "Model Number" visible (Y/N) | 0.08 hrs |
| Label photo: "Serial Number" visible (Y/N) | 0.08 hrs |
| Ensure installed correctly and free from damage | 0.08 hrs |
| Before energizing check power supply polarity and magnitude | 0.08 hrs |
| Remove power from device and confirm watchdog relay closes | 0.17 hrs |
| Device configuration completed. Ensure no errors occure | 1.5 hrs |
| Confirm correct VLAN configuration | 1.5 hrs |
| Confirm Time Zone | 0.08 hrs |
| Confirm DST | 0.08 hrs |
| Drawings marked up - Bluelines Completed | 1 hr |

**Final Activity (cannot start until all above complete):**
| Activity | Duration |
|----------|----------|
| QMS Entry | 0.33 hrs |

**Total Duration:** 5.48 hours

### -UC (Rectifiers)
| Activity | Duration | Relationship |
|----------|----------|--------------|
| Input / Outputs (Operation only) | 1.5 hrs | Parallel |
| Settings Configured | 30 mins | Parallel |
| Outgoing Distribution - Verified (One by One) | 30 mins | Parallel |
| Panel Bluelines Complete | 4 hrs | Parallel |

**Total Duration:** 6.5 hours

## Switchboard Equipment

### +WA CB (33kV Circuit Breakers)
| Activity | Duration | Relationship |
|----------|----------|--------------|
| Preparations & Setup | 2 hrs | Parallel |
| Record Ratings and Nameplate information | 30 mins | Parallel |
| Test Card setup | 2 hrs | Parallel |
| Leads | 30 mins | Parallel |
| Field Tests | 2 hrs | Parallel |
| Reporting | 1 hr | Parallel |

**Total Duration:** 8 hours

### +WA CT (33kV Current Transformers)
| Activity | Duration | Relationship |
|----------|----------|--------------|
| Resistance - Overall | 10 mins | Parallel |
| All power and earth cables re-connected | 5 mins | Parallel |
| All test links, bridging and wiring left in -service- position | 1 min | Parallel |
| Polarity of CT correct (By Primary Injection) | 5 mins | Parallel |
| CT single point earthing confirmed | 5 mins | Parallel |
| Drawings marked up - Bluelines Completed | 10 mins | Parallel |

**Total Duration:** ~36 minutes (0.6 hours)

### +WA VT (Voltage Transformers)
| Activity | Duration | Relationship |
|----------|----------|--------------|
| Nameplate details & Photo | 5 mins | Parallel |
| Serial No. | 5 mins | Parallel |

**Total Duration:** 10 minutes (0.166 hours)

## Power Systems

### +GB (DC Systems/Batteries)
| Activity | Duration | Relationship |
|----------|----------|--------------|
| SWMS Reviewed and finalised | 2 hrs | TBD |
| Boost Charge Completed | 24 hrs | TBD |
| Temporary Cabling installed | 30 mins | TBD |
| Discharge Test - CLIENT WITNESS | 8 hrs | TBD |
| Report and Paperwork | 2 hrs | TBD |

**Total Duration:** 36.5 hours  
**Note:** Relationship logic pending - likely sequential

---

# SAT Activities Library

## Pre-Energization Phase

### Pre-energization Activities
*Applied before any equipment energization*

| Activity | Duration | Relationship |
|----------|----------|--------------|
| Tightness | 20 hrs | TBD |
| Ductor | 20 hrs | TBD |
| IR | 4 hrs | TBD |
| Links Closed and in service | 4 hrs | TBD |
| Single Point Earthing | 4 hrs | TBD |

**Total Duration:** 52 hours  
**Note:** Relationship logic pending

## Energization Phase

### Energization Activities
| Activity | Duration | Relationship |
|----------|----------|--------------|
| Switching Procedure reviewed and approved | 0 hrs | Milestone |
| Switching | 4 hrs | Work |

**Total Duration:** 4 hours  
**Relationship:** Procedure approval → Switching

## Equipment-Specific SAT

### +WA 33kV (HV Switchboards)
*All milestone activities (0 hrs) that must complete before energization:*

| Activity | Duration | Type |
|----------|----------|------|
| Category A & B Punchlist items closed | 0 hrs | Milestone |
| Rotational devices de-Coupled (as required) | 0 hrs | Milestone |
| Construction verification complete | 0 hrs | Milestone |
| Construction Redpen drawings compiled | 0 hrs | Milestone |
| Certificate of Compliance Issued (as required) | 0 hrs | Milestone |
| NOE checklist completed and signed (Table 1‑5) | 0 hrs | Milestone |
| Supporting documents appended to checklist & controlled | 0 hrs | Milestone |
| Notice issued to key stakeholders 24 hrs (minimum) prior | 0 hrs | Milestone |
| Announced at pre-start on the day of energisation | 0 hrs | Milestone |
| Isolations applied (as required) | 0 hrs | Milestone |
| Area barricaded off and entry subject to controls | 0 hrs | Milestone |

**Total:** 11 milestone activities

### -F11 SAT
| Order | Activity | Duration |
|-------|----------|----------|
| 1 | Input / Output / Schematic Blueline Drawings | 2 hrs |
| 2 | 61850 - END-END | 2 hrs |

**Total Duration:** 4 hours  
**Relationship:** Sequential (drawings must complete before 61850 testing)

### -KF SAT
| Activity | Duration |
|----------|----------|
| Input / Output / Schematic Blueline Drawings | 2 hrs |

**Total Duration:** 2 hours

## System Integration

### SCADA Points Testing
- **WBS Level:** One level under SAT
- **Duration:** 127 hours total allocation
- **Activities:** Job-specific (not predefined)
- **Critical Dependency:** Cannot start until ALL FAT activities are complete
- **Scope:** Complete system integration testing

## Milestone Structure

### Transport Milestones
*Automatically generated for each subsystem:*

| Subsystem | Milestones |
|-----------|------------|
| Switchroom | Switchroom Ready for Transport |
|  | Switchroom Transport |
| 33/11kV Substation | 33/11kV Substation Ready for Transport |
|  | 33/11kV Substation Transport |

**Relationship:** FAT Complete → Ready for Transport → Transport → SAT begins

---

# Implementation Notes

## Relationship Logic Patterns

### Equipment Hierarchy Dependencies
```
+UH Activities → Child Equipment (-F, -KF, -Y, -UC)
```

### Milestone Gates
- **Zero Duration Activities:** Documentation/approval gates
- **QMS Activities:** Final quality checkpoints  
- **Prerequisites:** Must complete before work begins
- **Sequential Activities:** Specific order required
- **Parallel Activities:** Any order acceptable

### Critical Dependencies
- **SCADA Testing:** Requires ALL FAT complete
- **SAT Activities:** Require energization milestones
- **Transport:** Requires subsystem FAT complete

## Data Structure Requirements

### Activity Generation Logic
1. **Parse WBS structure** to identify equipment types
2. **Map equipment patterns** to activity templates
3. **Generate unique activity IDs** with WBS linkage
4. **Create relationships** based on equipment hierarchy
5. **Apply duration allocations** per equipment type
6. **Generate P6-compatible export**

### Equipment Recognition Patterns
- **+UH:** Protection Panels
- **+WA:** HV/LV Switchboards  
- **+WC:** LV Switchboards
- **T:** Transformers
- **+CA:** Ancillary Systems
- **+GB:** DC Systems
- **-F, -KF, -Y, -UC:** Child equipment with specific parents

## Future Development

### Pending Activity Sets
- **Transformers (T01/T02):** Major equipment testing
- **+WC (LV Switchboards):** Panel-level activities
- **System Integration:** End-to-end testing beyond SCADA
- **Additional SAT equipment:** Complete coverage

### Enhancement Opportunities
- **Resource assignment logic:** Equipment-specific crew allocation
- **Duration scaling:** Based on equipment size/complexity
- **Relationship optimization:** Parallel processing opportunities
- **Template variations:** Different project types/standards

---

*This README represents the current state of the Activities Generator development as of the data collection phase. All activity patterns, relationships, and durations are captured to enable successful implementation of the automated activity generation system.*
