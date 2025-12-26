---
layout: default
nav_exclude: true
---

# Advanced Traffic Light Control System with Emergency Preemption

> **Technologies:** Rockwell Studio 5000 Logix Designer · ControlLogix 5570 Emulator  
> **Key Concepts:** State Machine Design · Time-of-Day Scheduling · Emergency Vehicle Preemption · Pedestrian Safety · Conflict Detection

---

##  Overview

A **comprehensive traffic intersection control system** implementing intelligent signal timing, emergency vehicle preemption, pedestrian crosswalk management, and safety conflict detection — built and validated entirely in **Studio 5000** with the **ControlLogix 5570 Emulator**.

This project demonstrates professional traffic control system design with real-world features including adaptive timing based on time-of-day, emergency vehicle priority routing, pedestrian walk signals with countdown timers, and critical safety interlocks.

---

##  Project Highlights

- **Multi-Mode Operation** — Auto/Manual/Night modes with seamless transitions and safe sequencing
- **Time-of-Day Scheduling** — Normal, Rush Hour (NS), Rush Hour (EW), and Night Mode with adaptive green times
- **Emergency Vehicle Preemption** — North-South and East-West emergency detection with immediate priority clearance
- **Pedestrian Safety System** — Walk/Don't Walk signals with flashing countdown and timing coordination
- **Conflict Detection** — Real-time monitoring prevents simultaneous green signals with automatic safety beacon activation
- **Modular State Machine** — Clean 6-state cycle: NS Green → NS Yellow → All Red → EW Green → EW Yellow → All Red
- **Comprehensive Documentation** — Fully commented ladder logic with clear routine organization

---

##  System Architecture

The controller implements a sophisticated state machine with integrated safety systems:

### Operating Modes

**Auto Mode**
- State-driven traffic signal sequencing
- Time-of-day adaptive green times
- Pedestrian request handling
- Emergency vehicle preemption
- Conflict detection and safety interlocks

**Manual Mode**
- Direct operator control of NS/EW green signals
- Force all-red capability for maintenance
- Independent signal testing
- State timer suspension

**Night Mode** (11:00 PM - 6:00 AM)
- Flashing yellow (NS direction)
- Flashing red (EW direction)
- 1-second flash cycle
- Reduced power operation

### Time-of-Day Schedules

| Period | Mode | NS Green | EW Green | Use Case |
|--------|------|----------|----------|----------|
| 11 PM - 6 AM | Night Mode | Flashing Yellow | Flashing Red | Low traffic hours |
| 6 AM - 9 AM | Rush Hour (NS) | 60 seconds | 45 seconds | Morning commute |
| 9 AM - 4 PM | Normal | 45 seconds | 45 seconds | Standard operation |
| 4 PM - 7 PM | Rush Hour (EW) | 45 seconds | 60 seconds | Evening commute |
| 7 PM - 11 PM | Normal | 45 seconds | 45 seconds | Evening traffic |

---

##  Software Architecture

The system is organized into modular routines for maintainability and clarity:

| Routine | Purpose |
|---------|---------|
| **MainRoutine** | Entry point; orchestrates all subsystem calls |
| **R01_FIRST_SCAN** | System initialization and startup configuration |
| **R02_ToD_State** | Time-of-day mode selection and clock management |
| **R03_Timer_Presets** | Dynamic timer configuration based on operating mode |
| **R04_Emergency_Mode** | Emergency vehicle detection and preemption logic |
| **R05_Process_Logic** | Core state machine and signal timing control |
| **R06_Night_Mode** | Flashing signal control for night operation |
| **R07_Pedestrian_Handling** | Walk signal requests, countdown, and flash warnings |
| **R08_Lights_MappingOutputs** | Output arbitration and priority resolution |
| **R09_Manual_Mode** | Manual override controls and operator interface |

### Key Design Patterns

**State Machine Implementation**
- 6-state sequential control
- Timer-based state transitions
- Emergency state injection (States 7-8)
- Return-to-previous-state capability

**Output Arbitration Priority**
```
Emergency Mode > Manual Mode > Night Mode > Auto Mode
```

**Safety Interlocks**
- Conflict detection prevents NS Green + EW Green simultaneously
- All-red buffer periods between opposing green phases
- Safety beacon activation on detected conflicts
- Manual system reset required after conflict

---

##  Traffic Signal State Machine

### Normal Operation Cycle

```
State 1: NS Green (45-60s)    →  State 2: NS Yellow (4s)
    ↑                                      ↓
State 6: All Red (2s)         ←  State 3: All Red (2s)
    ↑                                      ↓
State 5: EW Yellow (4s)       ←  State 4: EW Green (45-60s)
```

### Emergency Preemption Sequence

```
Any State → State 7: All Red (2s) → State 8: Emergency Green (15s) → Return to Previous State
```

**Emergency Logic:**
- NS Emergency: State 8 = NS Green, EW Red
- EW Emergency: State 8 = EW Green, NS Red
- System stores previous state for return
- All timers reset during emergency
- Pedestrian signals inhibited

---

##  Pedestrian Control System

### Features

**Walk Signal Timing**
- 7-second WALK phase (solid illumination)
- 10-second flashing DON'T WALK countdown
- Total pedestrian clearance: 17 seconds

**Request Handling**
- Button press registers request
- Request serviced when traffic direction has green
- Automatic request clearing after walk cycle
- Emergency override prevents walk activation

**Countdown Flash Implementation**
- 500ms on/off flash cycle
- 9 flash cycles during countdown
- Counter-based flash control
- Synchronized with traffic state

**Safety Integration**
- Pedestrian time subtracted from green time
- Prevents late-stage crossing requests
- Walk only activates during appropriate traffic phase
- Emergency vehicles override pedestrian signals

---

##  Safety Systems

### Conflict Detection

**Monitoring Logic**
```
IF (Auto_NS_Green AND Auto_EW_Green) THEN
    Conflict_Trigger = TRUE
    Signal_Conflict = LATCH
    Safety_Beacon = ON
    Force State = All Red
END IF
```

**Recovery Procedure**
1. Conflict detected and latched
2. Safety beacon activated
3. System forced to All Red (State 3)
4. Manual system reset required
5. Operator investigation mandatory

### All-Red Clearance Intervals

- **2-second clearance** between opposing green phases
- Allows intersection clearing
- Prevents "trapped" vehicles
- Meets traffic engineering standards

---

##  Project Documentation

### 1. Project Summary

<embed src="ProjectSummary_TrafficLightControl.pdf" width="100%" height="600px" type="application/pdf">

**Contents:**
- System objectives and scope
- Operating mode descriptions
- State machine architecture
- Safety system overview
- Testing methodology

### 2. Complete Ladder Logic

<embed src="LadderLogic_Complete.pdf" width="100%" height="600px" type="application/pdf">

**Contents:**
- All 9 routine implementations
- State machine logic (R05_Process_Logic)
- Emergency preemption (R04_Emergency_Mode)
- Pedestrian control (R07_Pedestrian_Handling)
- Output arbitration (R08_Lights_MappingOutputs)
- Time-of-day scheduling (R02_ToD_State)

### 3. Tag Listing & Data Structures

<embed src="TagListing_DataStructures.pdf" width="100%" height="600px" type="application/pdf">

**Contents:**
- Controller-scoped tags
- User-defined data types (Traffic_Light_UDT, RTC)
- Timer configurations
- State variables
- I/O mapping

---

##  Technical Implementation Details

### User-Defined Data Types

**Traffic_Light_UDT**
```
- Red                : BOOL
- Yellow             : BOOL  
- Green              : BOOL
- Vehicle_Detected   : BOOL
- Emergency_Active   : BOOL
- Green_Time_Preset  : DINT
- Green_Time_Actual  : DINT
```

**RTC (Real-Time Clock)**
```
- Year         : DINT
- Month        : DINT
- Day          : DINT
- Hour         : DINT
- Minutes      : DINT
- Seconds      : DINT
- Micro_Seconds: DINT
```

### Timer Architecture

| Timer | Preset | Purpose |
|-------|--------|---------|
| State1_Timer | 45000-60000ms | NS Green duration |
| State2_Timer | 4000ms | NS Yellow duration |
| State3_Timer | 2000ms | All Red buffer (NS→EW) |
| State4_Timer | 45000-60000ms | EW Green duration |
| State5_Timer | 4000ms | EW Yellow duration |
| State6_Timer | 2000ms | All Red buffer (EW→NS) |
| State7_Timer | 2000ms | Emergency All Red |
| State8_Timer | 15000ms | Emergency Green |
| NS_Walk | 7000ms | Pedestrian WALK |
| FDW_NS | 10000ms | Flashing DON'T WALK |
| Night_Flash_Timer | 1000ms | Night mode flash cycle |

---

##  Testing & Validation

### Test Coverage

**Functional Tests**
- ✅ State machine sequencing (all 6 states)
- ✅ Timer preset adjustments per mode
- ✅ Emergency NS preemption
- ✅ Emergency EW preemption
- ✅ Emergency return-to-previous-state
- ✅ Pedestrian request NS direction
- ✅ Pedestrian request EW direction
- ✅ Pedestrian flash countdown
- ✅ Night mode flashing operation
- ✅ Manual mode NS green
- ✅ Manual mode EW green
- ✅ Manual mode all red

**Safety Tests**
- ✅ Conflict detection activation
- ✅ Safety beacon trigger
- ✅ Force all-red on conflict
- ✅ Prevent simultaneous green
- ✅ Emergency override of pedestrian
- ✅ All-red clearance intervals

**Time-of-Day Tests**
- ✅ Normal mode timing (9 AM - 4 PM)
- ✅ Rush hour NS (6 AM - 9 AM)
- ✅ Rush hour EW (4 PM - 7 PM)
- ✅ Night mode activation (11 PM - 6 AM)
- ✅ Mode transition handling

### Emulator Testing

All testing performed using:
- **ControlLogix 5570 Emulator**
- **Studio 5000 v33**
- **No physical I/O required**
- Complete functional validation without hardware investment

---

##  System Performance

### Timing Specifications

- **State Transition Time:** <50ms
- **Emergency Response:** <100ms from detection
- **Conflict Detection:** Real-time, every scan
- **Flash Cycle Accuracy:** ±10ms
- **Timer Resolution:** 1ms

### Scalability

**Current Implementation:**
- 4-way intersection (N/S/E/W)
- 16 digital outputs (4 lights × 3 colors + pedestrian)
- 2 digital inputs (pedestrian buttons)
- 2 emergency vehicle inputs

**Expansion Capability:**
- Additional intersection phases
- Turn arrow signals
- Vehicle detection loops
- Adaptive timing algorithms
- Network communications
- Traffic data logging

---

##  Learning Outcomes

This project demonstrates mastery of:

**PLC Programming Concepts**
- State machine design and implementation
- Timer-based sequencing and control
- Priority-based output arbitration
- Interlocking and safety logic

**Traffic Control Systems**
- Signal timing and phasing
- Emergency vehicle preemption
- Pedestrian safety integration
- Time-of-day scheduling

**Software Engineering**
- Modular routine organization
- Maintainable code structure
- Comprehensive documentation
- Systematic testing methodology

**Industrial Standards**
- Safety-critical system design
- Conflict detection requirements
- Clearance interval implementation
- Manual override capabilities

---


---

##  Download Project Files

**[Complete Studio 5000 Project (.ACD)](./Traffic_Light_Control.ACD)** — Import-ready project file

---

[🔙 Back to Projects](../../projects)