# ⚙️ How Everything Connects at Runtime

## 🧠 Overview

This document explains how all systems — diagnosis selection, behavior execution, indicator spawning, FX, temperature feedback, and parameter updates — are **coordinated at runtime**.

The central class is `UCPP_SimulationManager`. It owns the simulation state and delegates specific responsibilities to modular subsystems:
- `UCPP_Diagnosis` for static diagnosis data
- `UShockBehavior` for diagnosis-specific logic
- Niagara FX, breathing dot indicators, and floating temperature displays for visual output

---

## 🧩 Initialization Phase

### 1. `UCPP_SimulationManager::BeginPlay()`

- Instantiates `UDiagnosisRegistery`
- Calls `Initialize(this)` to ensure correct UObject ownership
- Diagnosis Registery builds all available diagnoses using `DiagnosisBuilder`

### 2. `UDiagnosisRegistery::BuildDiagnosisList()`

For each diagnosis:
- Sets metadata: name, type, description
- Sets default simulation values via `FSimulationSlideBarsParameters`
- Registers Niagara FX: `.AddEffect()`
- Registers indicators: `.AddIndicator()`
- Enables temperature feedback: `.SetTemperatureDisplay()`
- Registers behavior logic: `.SetBehavior(NewObject<UYourShockBehavior>())`

Example:

```cpp
DiagnosisBuilder(this)
  .SetName("Cardiogenic Shock")
  .SetSimulationParameters(Params)
  .AddEffect(NS_Sweat, true, "Forehead")
  .AddIndicator("Wrist", "Pulse", "Weak pulse", Icon)
  .SetBehavior(NewObject<UCardiogenicShockBehavior>(this))
  .SetTemperatureDisplay(true, 36.5f, 30.0f, "spine_03", "Wrist")
  .Build();
```

These instances are stored in a `TArray<TUniquePtr<UCPP_Diagnosis>> DiagnosisList`.

---

## 🔁 Changing Diagnosis

Called by UI or developer logic to switch medical scenarios.

### 1. `UCPP_SimulationManager::ChangeDiagnosis(EDiagnosisType NewDiagnosis)`

- Calls `CurrentShockBehavior->OnExit()` to clean up:
  - Destroyed FX components
  - Cleared indicators
  - Removed floating labels
- Retrieves new `UCPP_Diagnosis*` from registery
- Stores it in `ActiveDiagnosis`
- Assigns its `RuntimeBehavior` as the new `CurrentShockBehavior`
- Calls `CurrentShockBehavior->OnEnter(ActiveDiagnosis, this)`:
  - FX are spawned based on diagnosis
  - Pulse timers or parameter logic begin
  - Floating temperatures shown if enabled
- Spawns breathing dot widgets via `SpawnIndicators()`
- Broadcasts `FChangeDiagnosis` delegate (for UI updates)

---

## 📊 Updating Simulation

Simulation parameters are changed in real-time by UI sliders (Speed, BPM, Thickness, etc.).

### 2. `UCPP_SimulationManager::UpdateSimulation(FSimulationSlideBarsParameters* Params)`

- Updates internal `SimulationParameters`
- Calls `CurrentShockBehavior->OnUpdate(Params)`
  - May change pulse timing, FX intensity, label data, etc.
- Broadcasts `FUpdateSimulation` delegate

Example from Cardiogenic Shock:

```cpp
void UCardiogenicShockBehavior::OnUpdate(FSimulationSlideBarsParameters* Params) {
  float Interval = GetPulseInterval(Params->BeatsPerMinute);
  StartPulseTimer(Interval); // Controls Niagara pulse FX
}
```

---

## 🛑 Stopping Simulation

Called when user exits, resets, or changes context.

### 3. `UCPP_SimulationManager::StopSimulation()`

- Calls `CurrentShockBehavior->OnExit()`
- Clears all FX, indicators, and temperature widgets
- Broadcasts `FStopSimulation`

---

## 🔄 Runtime Behavior Summary

```mermaid
graph TD
  A[BeginPlay] --> B[DiagnosisRegistery -> BuildDiagnosisList]
  B --> C[Each Diagnosis Configured]
  C --> D[Ready to Simulate]

  D --> E[ChangeDiagnosis(Type)]
  E --> F[Clear Previous Behavior + FX]
  F --> G[Load New Diagnosis]
  G --> H[Assign RuntimeBehavior]
  H --> I[Call OnEnter()]
  I --> J[Spawn FX, Indicators, Temp Labels]
  J --> K[Broadcast Change Event]

  K --> L[UpdateSimulation()]
  L --> M[OnUpdate() modifies timers/FX]
  M --> N[Looping Runtime Behavior]

  N --> O[StopSimulation()]
  O --> P[Cleanup + Broadcast Stop]
```

---

## 💡 Key Class Responsibilities

| Class                  | Role                                                           |
|------------------------|----------------------------------------------------------------|
| `UCPP_SimulationManager` | Orchestrates full simulation lifecycle                        |
| `UCPP_Diagnosis`       | Stores static diagnosis data                                   |
| `UShockBehavior`       | Executes runtime logic like timers, FX, and state changes      |
| `UDiagnosisRegistery` | Owns and builds all diagnosis entries                          |
| `UShockIndicator`      | Represents each breathing dot data                             |
| `UBreathingDotWidget`  | Visual UI representation of an indicator                       |
| `FloatingTemperatureLabel` | Displays core and skin temp near anatomy                   |

---

## ✅ Tips for Developers

- **Encapsulation**: Avoid placing diagnosis logic in `SimulationManager`. Always delegate via `UShockBehavior`.
- **Extendability**: Create new `UShockBehavior` subclasses for every new diagnosis.
- **Cleanup**: Always implement `OnExit()` properly to avoid ghost widgets or FX leaks.
- **Testing**: Toggle between diagnosis types rapidly to validate correct cleanup and re-entry behavior.

---
