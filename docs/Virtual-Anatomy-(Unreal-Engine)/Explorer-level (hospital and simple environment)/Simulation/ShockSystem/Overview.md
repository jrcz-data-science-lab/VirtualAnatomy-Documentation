# Shock System Overview

Welcome to the **Shock System** documentation for the Virtual Anatomy project. This system is a core component responsible for simulating various medical diagnoses and providing real-time visual and interactive feedback to the user.

At its heart, the Shock System orchestrates dynamic changes within the anatomical model, including:
* Activation and management of specific medical diagnoses (e.g., Cardiogenic Shock).
* Dynamic updates of simulation parameters (like heart rate, blood pressure, etc.).
* Display of crucial visual indicators, such as "breathing dots" and floating temperature labels.
* Integration of sophisticated Niagara visual effects (FX) to enhance realism.

This system is designed to be highly modular and extensible, allowing for the easy addition of new diagnoses and associated behaviors.

---

## Core Components

The Shock System is built around several interconnected C++ classes:

* **`UCPP_SimulationManager`**: The central orchestrator. It manages the overall simulation state, delegates behavior to specific diagnosis handlers, and controls the spawning and clearing of all visual indicators and FX.
* **`UDiagnosisRegistery`**: A repository that stores and provides access to all predefined `UCPP_Diagnosis` profiles. It uses `DiagnosisBuilder` to construct these profiles.
* **`UCPP_Diagnosis`**: Represents a single, complete medical diagnosis. Each instance encapsulates all the data and configuration for a specific shock type, including simulation parameters, FX, UI indicators, and optional runtime behavior (`UShockBehavior`).
* **`UShockBehavior`**: (Base class) Defines the specific runtime logic and actions for a given diagnosis. Subclasses of `UShockBehavior` (e.g., `UCardiogenicShockBehavior`, `UDistributiveShockBehavior`, `UObstructiveShockBehavior`) implement the unique characteristics of each medical condition.
* **`DiagnosisBuilder`**: A utility class used by `UDiagnosisRegistery` to fluently construct `UCPP_Diagnosis` objects with all their associated properties, effects, and indicators.
* **`UBreathingDotWidget`**: A `UUserWidget` subclass used to create dynamic, animated UI indicators that typically represent physiological points or interactive hotspots.
* **`AFloatingTemperatureLabel`**: An `AActor` responsible for displaying 3D text labels for core and skin temperatures in the world.

---

## Capabilities

The Shock System enables:
* **Dynamic Diagnosis Switching**: Seamlessly change between different medical conditions at runtime.
* **Real-time Parameter Updates**: Adjust simulation parameters (e.g., speed, BPM) during an active simulation.
* **Visual Feedback**:
    * **Breathing Dots**: Intuitive UI elements for showing vital signs or interactive regions.
    * **Niagara FX**: High-fidelity particle effects for visual representation of physiological states (e.g., sweat, pulse, internal conditions).
    * **Temperature Labels**: 3D floating text displaying core and skin temperatures.
* **Extensible Behavior**: Easily add new diagnosis types with unique runtime behaviors without modifying core system logic.

---

## Sections in Detail

| Topic                                | Description                                                               | Relevant Classes                             |
| :----------------------------------- | :------------------------------------------------------------------------ | :------------------------------------------- |
| [[Breathing Dots]]                 | Detailed look at `UBreathingDotWidget` and `UShockIndicator`.             | `UBreathingDotWidget`, `UShockIndicator`     |
| [[FX and Materials]]               | Configuration of Niagara Effects (`FDiagnosisEffect`) and their use.    | `FDiagnosisEffect`, `UNiagaraSystem`         |
| [[How Everything Connects at Runtime]] | The interaction flow between `UCPP_SimulationManager`, `UCPP_Diagnosis`, `UDiagnosisRegistery`, and `UShockBehavior`. | `UCPP_SimulationManager`, `UCPP_Diagnosis`, `UDiagnosisRegistery`, `UShockBehavior` |
| [[Debugging & Extension Tips]]     | Guidance on troubleshooting and extending the Shock System.             | (General, applies to all classes)            |

---

Continue to the next sections for an in-depth exploration of each component.