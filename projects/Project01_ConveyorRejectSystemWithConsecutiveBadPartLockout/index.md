---
layout: default
nav_exclude: true
---

# 📦 Project 1: Conveyor Reject System with Consecutive Bad Part Lockout

This project simulates a real-world reject conveyor using **Rockwell Studio 5000** and **RSLogix Emulate 5570**. The system automatically tracks and rejects bad parts using diverter logic and halts the conveyor after 10 consecutive rejections. Fault reset logic ensures safe operator recovery. It's a modular, scan-based design fit for packaging, inspection, and pharma automation lines.

---

## 🧠 System Overview

- Start/Stop logic using pushbuttons with seal-in structure
- Simulated part sensor and manual bad part detection
- Diverter pulse control for rejecting bad parts
- CTU and RES instructions track consecutive bad parts
- Conveyor stops automatically on 10 consecutive rejects (`Batch_Fault`)
- Manual fault reset through `Sys_Reset`
- Built using structured routines: `Map_IO`, `Process_Logic`, `Faults_and_Reset`

---

## 📄 Project Report

### 📘 Summary Documentation  
Explains logic flow, tag structure, fault control, and scan behavior.

<embed src="ProjectSummary_ConveyorRejectSystem.pdf" width="100%" height="600px" type="application/pdf">

[📥 Download Summary PDF](ProjectSummary_ConveyorRejectSystem.pdf)

---

### 💡 Ladder Logic Overview  
Screenshots of seal-in, diverter pulse, reject counter, and fault lockout.

<embed src="ConveyorRejectSystem_LadderLogic.pdf" width="100%" height="600px" type="application/pdf">

[📥 Download Logic PDF](ConveyorRejectSystem_LadderLogic.pdf)

---

## 🖼️ Preview

### Conveyor Running Normally  
![Normal Conveyor Mode](Conveyor1.png)

### Fault Active After 10 Bad Parts  
![Batch Fault Triggered](Conveyor2.png)

---

[🔙 Back to Projects](../../projects)

