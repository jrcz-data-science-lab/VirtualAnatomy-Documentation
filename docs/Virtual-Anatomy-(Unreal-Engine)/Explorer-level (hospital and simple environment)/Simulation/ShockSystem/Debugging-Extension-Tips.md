# 🧪 Debugging & Extension Tips

## Overview

This section provides **practical guidance** on how to debug and expand the Shock Simulation system safely and effectively — whether you're working in **C++**, **Blueprints**, or both.

The architecture is modular and scalable but also layered. This means debugging requires you to understand where data flows and where it breaks. Extension must respect ownership, lifecycles, and modular separation of logic.

---

## 🔍 General Debugging Workflow

When something doesn’t work (e.g. dots don’t show up, FX doesn’t spawn, diagnosis logic fails), try this top-down approach:

1. **Is the right diagnosis being triggered?**
   - Check `SimulationManager::ChangeDiagnosis()`
   - Use `UE_LOG()` to confirm `SelectedDiagnosis->GetName()`

2. **Does the diagnosis have the right components?**
   - Use breakpoints/logs to confirm:
     - `Indicators` array populated
     - `RuntimeBehavior` is not null
     - `FX` list is not empty

3. **Is the component being spawned?**
   - Breathing Dots: `UBreathingDotWidget`
   - Niagara FX: via socket positions
   - Temp Labels: `AFloatingTemperatureLabel`

4. **If it's a UI issue:**
   - Use **Blueprint breakpoints** in the widget
   - Confirm `SetDescription()` is called
   - Log Widget creation (`UE_LOG()` or use `PrintString()`)

5. **If it’s a 3D issue:**
   - Use `DrawDebugSphere()` or `DrawDebugString()` to visualize positions

---

## 🧠 Logging & Debugging Utilities

### C++ Logging

```cpp
UE_LOG(LogTemp, Warning, TEXT("FX Spawned at: %s"), *SpawnLocation.ToString());
UE_LOG(LogTemp, Display, TEXT("Diagnosis: %s"), *Selected->GetName());
```

### Blueprint Debugging

Use:
- `PrintString` nodes
- Breakpoints in:
  - `UBreathingDotWidget`
  - `WBP_TemperatureDisplay`
  - `UCPP_DiagnosisDropDownMenu`

---

## 🎯 Tips for Common Problems

| 🧪 Problem                        | ✅ Solution                                                        |
|----------------------------------|--------------------------------------------------------------------|
| Indicators not showing           | Socket name typo? Mesh doesn't contain it? Use `DoesSocketExist()` |
| FX not appearing                 | Niagara System valid? Socket exists? FX destroyed too early?       |
| Widget not displaying            | Widget created? Anchor within screen bounds?                       |
| Temp label not visible           | Is display enabled? Core/skin sockets defined?                     |
| OnEnter() not called             | Is `SetBehavior()` actually set in the builder?                    |
| Crash on change diagnosis        | Null pointer? FX or indicator called before fully built?           |

---

## 🛠 Extension Tips

### 🔁 Adding New Behavior Logic

Create a new subclass of `UShockBehavior`, then plug it into your diagnosis:

```cpp
class UMyCustomShockBehavior : public UShockBehavior {
  virtual void OnEnter() override { ... }
  virtual void Tick(float DeltaTime) override { ... }
  virtual void OnExit() override { ... }
};
```

```cpp
DiagnosisBuilder(this)
  .SetBehavior(NewObject<UMyCustomShockBehavior>(this))
  .Build();
```

This allows per-frame logic unique to that diagnosis.

---

### 🌬️ Add Custom Niagara FX

1. Create Niagara FX in Content Browser.
2. Add to the builder:

```cpp
.AddEffect(NS_PulseFX, true, FName("spine_03"))
```

3. FX will automatically:
   - Spawn on diagnosis enter
   - Be destroyed on diagnosis change

Use `FDiagnosisEffect` to customize:

```cpp
FDiagnosisEffect(NS_SweatFX, true, "Forehead");
```

---

### 📌 Add New Indicator Type

If you want to make a new type of UI indicator (e.g. icon overlay, status chip):

1. Create a new UMG widget (e.g. `WBP_AlertIcon`)
2. Make a new data class (e.g. `UCustomShockIndicator`)
3. Extend spawn logic in `SimulationManager`:
   - Use `WidgetComponent->SetWidgetClass()`
   - Set data via Blueprint function call (`SetData()`)

---

## 🧠 Advanced Debugging (Runtime Visuals)

### Debug Niagara Locations

```cpp
DrawDebugSphere(GetWorld(), Mesh->GetSocketLocation(SocketName), 10.0f, 12, FColor::Red, false, 5.0f);
```

### Trace Tick Lifecycle

If `Tick()` in `UShockBehavior` isn’t being called:
- Check `PrimaryComponentTick.bCanEverTick = true`
- Ensure you're calling `RegisterComponent()` on it
- Confirm `SimulationManager` isn’t manually skipping it

---

## 🧪 Tools & Shortcuts

| Tool             | Use Case                                      |
|------------------|-----------------------------------------------|
| `UE_LOG()`       | General C++ debug output                      |
| `DrawDebugSphere`| Visualize positions for FX and UI             |
| Blueprint Logs   | Test data passed to widgets                   |
| Live Blueprint Debugger | Step through widget lifecycle         |
| Output Log       | Trace startup and runtime events              |

---

## 🧱 Dev Recommendations

- Centralize all changes inside **DiagnosisBuilder** to avoid logic leaks.
- Prefer **data-driven spawning** over hardcoded `if (DiagnosisType == X)` checks.
- When extending UI, keep logic in widgets and only inject **data** from C++.
- Clean up all spawned actors and widgets in **`ChangeDiagnosis()`** to avoid state leaks.
- Add yourself to the top of each diagnosis file (e.g., `// Modified by Kristers G. on 2025-06-17`)

---
