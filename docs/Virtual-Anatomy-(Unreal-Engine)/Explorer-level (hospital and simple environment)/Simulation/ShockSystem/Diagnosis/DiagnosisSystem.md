# 🧬 Diagnosis System

## Overview

> **NOTE**: This system uses advanced C++ principles. It assumes you're familiar with:
>
> - **Smart Pointers** (`TUniquePtr` and move semantics)
> - **Const References** (ensuring safe and immutable data access)

This system powers the disease and shock simulation in the Virtual Anatomy project. It defines diagnosis metadata (e.g., symptoms, parameters, FX, logic) that gets used in simulation runtime.

Currently supported diagnoses include:
- HyperVolumetric Shock
- Cardiogenic Shock
- Obstructive Shock
- Distributive Shock
- Death (as a diagnosis)

It is designed to be **extensible**, so more diagnoses can be added with minimal effort.

---

## 🧠 Core Design Principles

- All diagnoses are created and owned by `UDiagnosisRegistery`.
- They are stored as `TUniquePtr<UCPP_Diagnosis>` — enforcing **single ownership**.
- Only **const references** are returned to external systems (e.g. SimulationManager).
- Construction of each diagnosis is handled via a `DiagnosisBuilder`, using fluent method chaining.

> ⚠️ Never manually copy a `UCPP_Diagnosis` or break ownership with raw pointers.

---

## 📦 Components

### `UDiagnosisRegistery`
- Owns all `UCPP_Diagnosis` instances.
- Provides public `GetDiagnosisByType()` access.
- Initializes all diagnoses in `BuildDiagnosisList()`.

### `UCPP_Diagnosis`
- Encapsulates all diagnosis-related data:
  - Simulation parameters (`FSimulationSlideBarsParameters`)
  - Title + description
  - Enum type (`EDiagnosisType`)
  - FX (`TArray<FDiagnosisEffect>`)
  - UI Indicators (`TArray<UShockIndicator*>`)
  - Runtime logic class (`UShockBehavior*`)

### `DiagnosisBuilder`
- Used to create each diagnosis instance with a fluent interface.
- Enforces construction rules and prevents misuse.

---

## 🧾 Diagnosis Type Enum

The simulation system uses the `EDiagnosisType` enum to identify each diagnosis uniquely.

```cpp
UENUM(BlueprintType)
enum class EDiagnosisType : uint8
{
	Healthy = 0,
	HyperVolumetricShock,
	CardiogenicShock,
	ObstructiveShock,
	DistributiveShock,
	Death
};
```

This enum is used in dropdown selection, simulation flow, and builder logic.

---

## 📋 How to Add a New Diagnosis

### 1. Extend the Enum
In `Types/DiagnosisTypes.h`:
```cpp
enum class EDiagnosisType : uint8
{
	Healthy = 0,
	HyperVolumetricShock,
	// ...
	AnaphylacticShock // new
};
```

### 2. Add to UI Dropdown
In `UCPP_DiagnosisDropDownMenu::NativePreConstruct()`:
```cpp
DiseaseOptions.Add("Anaphylactic Shock", EDiagnosisType::AnaphylacticShock);
```

### 3. Build It in the Registery
In `UDiagnosisRegistery::BuildDiagnosisList()`:

```cpp
FSimulationSlideBarsParameters anaParams;
anaParams.Speed = 0.7f;
anaParams.BloodThickness = 200;
anaParams.BeatsPerMinute = 180;

auto newDiagnosis =
	DiagnosisBuilder(this)
		.SetName("Anaphylactic Shock")
		.SetDescription("Severe allergic reaction leading to shock")
		.SetDiagnosisType(EDiagnosisType::AnaphylacticShock)
		.SetSimulationParameters(anaParams)
		.AddIndicator("Neck", "Swelling", "Severe neck swelling", nullptr)
		.AddEffect(AllergyFX, true, "Head")
		.SetBehavior(NewObject<UAnaphylacticShockBehavior>(this))
		.Build();

DiagnosisList.Add(MoveTemp(newDiagnosis));
```

---

## 🔁 Runtime Flow

### Diagnosis Switching

When the user selects a diagnosis:

1. `UCPP_SimulationManager::ChangeDiagnosis()` is called.
2. Active FX and widgets are cleared.
3. New diagnosis is fetched from `UDiagnosisRegistery`.
4. All relevant indicators, FX, temp displays are spawned.
5. The `UShockBehavior` (if present) has `OnEnter()` called.
6. Changes are propagated via `FChangeDiagnosis` delegate.

### Example Listener

```cpp
void ACPP_BloodPathSystem::HandleDiagnosisChange(UCPP_Diagnosis& selectedDiagnosis)
{
	HandleSimulationUpdate(selectedDiagnosis.GetSimulationParameters());
	BloodParticleComponent->ReinitializeSystem();
}
```

---

## 🔬 Optional Extensions per Diagnosis

| Feature | Class |
|--------|-------|
| UI Dots | `UShockIndicator` |
| Niagara FX | `FDiagnosisEffect` |
| Runtime Logic | `UShockBehavior` subclass |
| 3D Temp Displays | `AFloatingTemperatureLabel` |

These components are optional. The builder allows you to skip any of them.

---

## 🗂 Class Summary

| Class | Role |
|-------|------|
| `UCPP_Diagnosis` | Holds one diagnosis's data |
| `UDiagnosisRegistery` | Owns all diagnoses |
| `DiagnosisBuilder` | Fluent builder for diagnosis setup |
| `UShockIndicator` | UI breathing dot anchor/socket |
| `FDiagnosisEffect` | Niagara FX setup per diagnosis |
| `UShockBehavior` | Runtime logic (tickable) |
| `AFloatingTemperatureLabel` | Floating 3D widget with temp values |

---

## 🧠 Developer Reminders

- ✅ Always use `DiagnosisBuilder` to create a diagnosis.
- ✅ Use `MoveTemp()` when adding to the list.
- ✅ Runtime logic should go into your `UShockBehavior` subclass.
- ⚠️ Never copy or leak `UCPP_Diagnosis` pointers.
- ⚠️ Socket names must match skeleton socket bones.

---
