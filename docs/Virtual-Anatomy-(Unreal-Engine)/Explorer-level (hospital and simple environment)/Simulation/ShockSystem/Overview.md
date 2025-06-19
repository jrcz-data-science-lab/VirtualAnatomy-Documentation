# 🧠 Shock System Overview

Welcome to the **Shock System** documentation for the Virtual Anatomy project.  
This system is the core engine responsible for simulating medical diagnoses and triggering all associated visuals, logic, and interactions.

At its core, the Shock System:

- Activates and manages specific medical diagnoses (e.g., Cardiogenic Shock)
- Updates simulation parameters in real-time (e.g., BPM, blood speed)
- Spawns breathing dot indicators and floating temperature labels
- Triggers Niagara FX based on diagnosis-specific behavior
- Delegates runtime logic to modular shock behavior classes

It is designed for **modularity**, **data-driven configuration**, and **runtime extensibility**, making it easy to add new diagnosis types or behaviors.

---

## 🧩 Core Architecture

The system is composed of the following major classes:

| Class                    | Purpose                                                                                   |
|--------------------------|-------------------------------------------------------------------------------------------|
| `UCPP_SimulationManager` | Central coordinator. Manages the active diagnosis, simulation parameters, FX, and UI.     |
| `UDiagnosisRegistery`    | Holds and constructs all available `UCPP_Diagnosis` objects using the `DiagnosisBuilder`. |
| `UCPP_Diagnosis`         | Represents a specific diagnosis (e.g., Cardiogenic Shock). Stores all relevant settings.  |
| `UShockBehavior`         | Runtime logic executor. Each diagnosis has its own subclass to handle behavior (e.g., FX).|
| `DiagnosisBuilder`       | Fluent API for constructing complete diagnoses, indicators, effects, and behavior.        |
| `UShockIndicator`        | Data structure defining a breathing dot’s socket, title, description, and icon.           |
| `UBreathingDotWidget`    | Widget used to visually represent indicators in the 3D space.                             |
| `AFloatingTemperatureLabel` | Actor that renders dynamic core and skin temperature data in the scene.              |

---

## 🚀 Capabilities

- **Dynamic Diagnosis Switching**
  - Switch between diagnosis types at runtime
  - Cleans up previous state and enters new behavior
- **Real-Time Simulation Updates**
  - BPM, blood speed, and viscosity are adjustable during runtime
- **Fully Visualized System**
  - **Breathing Dots**: UI elements bound to anatomical sockets
  - **Niagara FX**: Sweat, pulse, shock visuals, etc.
  - **Temperature Labels**: 3D feedback for skin and core temps
- **Extensible Runtime Logic**
  - Every diagnosis can have its own `UShockBehavior`
  - Runtime effects and timing are modular per behavior class

---

## 🔍 Docs Overview

| Section                            | Description                                                                 | Key Classes                               |
|------------------------------------|-----------------------------------------------------------------------------|-------------------------------------------|
| [Breathing Dots](Breathing-Dots.md) | Shows how UI indicators are defined in C++ and rendered in 3D.              | `UShockIndicator`, `UBreathingDotWidget`  |
| [FX and Materials](Fx-and-Materials.md) | Describes how Niagara effects are assigned and controlled.                | `FDiagnosisEffect`, `UNiagaraSystem`      |
| [How Everything Connects at Runtime](How-Everything-Connects-At-Runtime.md) | Step-by-step runtime execution of diagnosis switching and updates. | `UCPP_SimulationManager`, `UShockBehavior`, `UDiagnosisRegistery`, `UCPP_Diagnosis` |
| [Debugging & Extension Tips](Debugging-Extension-Tips.md) | Practical advice for adding features, fixing bugs, and scaling the system. | (Applies to all subsystems)               |

---

## ✅ Summary

The Shock System is the foundational layer that unifies diagnosis data, runtime behavior, visuals, and UI indicators.  
Thanks to its **data-driven structure** and **behavior-oriented logic**, adding new features or diagnoses requires **minimal changes to core code**.

This documentation suite will walk you through each part of the system.  
Start with [Diagnosis System](Diagnosis-System.md) to understand how everything begins.

