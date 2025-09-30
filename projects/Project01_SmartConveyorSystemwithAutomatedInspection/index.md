---
layout: default
nav_exclude: true
---


# Smart Conveyor System with Automated Inspection & Reject Handling

> **Technologies:** Rockwell Studio 5000 Logix Designer · ControlLogix 5570 Emulator  
> **Key Concepts:** Modular PLC Programming · UDTs & AOIs · FIFO Part Tracking · Alarm Handling

---

## 📌 Overview

A **multi-zone conveyor system** that detects, inspects, and automatically rejects defective parts — built and tested entirely in **Studio 5000** with the **ControlLogix 5570 Emulator** (no physical I/O required).  
The project models an industrial **inspection line** such as those used in packaging or assembly plants.

---

## 🎯 Highlights

- **Auto / Manual Modes** — safe start/stop sequencing and full manual jog control.  
- **Reusable Motor Control** — `AOI_ConveyorMotor` for each conveyor zone.  
- **FIFO Queue Tracking** — keeps part order from entry to exit.  
- **Automated Inspection** — checks label, seal, and weight; rejects failures.  
- **Scalable Alarm Handling** — modular `AOI_Alarm` with trip, latch, and acknowledge.  
- **Structured Codebase** — routines split for IO mapping, modes, conveyor control, inspection, FIFO, and alarms.

---

## 🧪 Testing

- Fully **simulated and validated** in the Studio 5000 Emulator.  
- Functional test matrix covers auto start/stop, inspection pass/fail, reject actuation, alarms, and manual jogging.  
- No physical chassis or wiring required — ideal for a learning environment and portfolio demonstration.

---

## 📂 Project Files

- [Project Overview]
<embed src="ProjectSummary_SmartConveyorSystemwithAutomatedInspection.pdf/" width="100%" height="600px" type="application/pdf">
(ProjectSummary_SmartConveyorSystemwithAutomatedInspection.pdf)

- [Ladder Logic]  
<embed src="LadderLogic.pdf/" width="100%" height="600px" type="application/pdf">
(LadderLogic.pdf)


- [AOIs]
<embed src="AOI.pdf/" width="100%" height="600px" type="application/pdf">
(AOI.pdf)

---

## 💡 Learning Outcomes

This project demonstrates **OEM/integrator-style PLC design** — modular, data-driven, and scalable — and shows ability to design, test, and document complete control systems without physical hardware.


[🔙 Back to Projects](../../projects)

