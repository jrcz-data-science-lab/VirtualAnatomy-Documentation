# 🫁 Breathing Dot Indicators

## Overview

Breathing Dots are dynamic UI widgets that visually mark **diagnosis-specific anatomical points** (e.g., a faint wrist pulse during Cardiogenic Shock). They:
- Animate with a subtle "breathing" pulse.
- Follow a 3D socket on the anatomical model.
- Display an **info panel** on hover or click with title, description, and optional image.
- Are **spawned dynamically** based on diagnosis configuration, and removed on diagnosis change.

They are implemented via:
- A C++ data class: `UShockIndicator`
- A Blueprint widget: `UBreathingDotWidget` (WBP_BreathingDot)
- Spawned/managed by: `UCPP_SimulationManager`

---

## 🧬 System Architecture

### Data Source: `UShockIndicator`

Each indicator is described by:

| Field        | Description                                              |
|--------------|----------------------------------------------------------|
| `SocketName` | Bone/socket name the indicator should follow (e.g. `"Wrist"`) |
| `Title`      | Title text for the info panel                            |
| `Description`| Tooltip-style detailed description                       |
| `Image`      | Optional image/texture shown in the info panel           |

Each `UCPP_Diagnosis` has an array:  
```cpp
TArray<UShockIndicator*> Indicators;
```

These are registered during diagnosis building using:

```cpp
.AddIndicator("Wrist", "Pulse", "Faint pulse detected", IconTexture)
```

---

### Runtime Spawning Logic

Inside `UCPP_SimulationManager::ChangeDiagnosis()`:

1. All previously spawned breathing dots are destroyed.
2. The `Indicators` array from the selected diagnosis is retrieved.
3. For each `UShockIndicator`, a `UBreathingDotWidget` is:
   - Created dynamically
   - Anchored to the corresponding bone/socket
   - Initialized via `SetDescription()` or `InitializeFromIndicator()` using `ProcessEvent`
   - Tracked in a local `TArray<UUserWidget*> SpawnedWidgets` for cleanup

---

### Widget: `UBreathingDotWidget`

This UMG Blueprint handles:
- **3D following**: Converts world position of the socket to screen position.
- **Visual pulsing**: Looping animation to draw user attention.
- **Info Panel logic**:
  - Tooltip-style display on hover or click
  - Receives title, description, and image from the indicator

Functions called from C++:
- `InitializeFromIndicator()` or `SetDescription(FString Title, FString Desc, UTexture2D* Image)`
- Toggle visibility or appearance based on current diagnosis

---

## 🔁 Example Builder Usage

Here’s how a diagnosis defines its breathing dots:

```cpp
DiagnosisBuilder(this)
  .SetName("Cardiogenic Shock")
  .SetDescription("Heart failure causing reduced blood circulation")
  .AddIndicator("Wrist", "Weak Pulse", "Pulse is faint and irregular", WristIcon)
  .AddIndicator("Chest", "Chest Discoloration", "Bluish skin near heart", ChestIcon)
  .Build();
```

---

## 🎯 Integration Map

| Class                     | Responsibility                                 |
|---------------------------|------------------------------------------------|
| `UShockIndicator`         | Stores metadata for one indicator              |
| `UCPP_Diagnosis`          | Holds multiple indicators                      |
| `UCPP_SimulationManager`  | Spawns/removes widgets based on diagnosis      |
| `UBreathingDotWidget`     | UMG widget shown on-screen for each dot        |

---

## 🧪 Debugging Breathing Dots

### If dots don’t appear:
- ✅ Check that `SocketName` exists on the skeletal mesh:
```cpp
SkeletalMesh->DoesSocketExist(FName("Wrist"))
```
- ✅ Log dot creation in C++:
```cpp
UE_LOG(LogTemp, Warning, TEXT("Spawning indicator for socket %s"), *SocketName.ToString());
```
- ✅ Use `DrawDebugSphere()` to confirm socket world location is valid.

### If info panel doesn’t show:
- ✅ Confirm that `SetDescription()` or `InitializeFromIndicator()` is called.
- ✅ Use breakpoints inside the Blueprint widget.
- ✅ Ensure image is valid or pass `nullptr`.

---

## 🔥 Advanced Features & Customization

### Add Conditional Visibility
Inside `UBreathingDotWidget`, use Blueprint logic:
- Only show info panel when hovered.
- Auto-hide based on diagnosis-specific rules.
- Support alternate states (e.g., blinking, fading).

### Override Appearance Per Dot
Each indicator could eventually have:
- A custom **color theme**
- A **different pulse rate** or animation
- Even **contextual behavior** depending on vital stats (BPM, blood volume)

Just extend `UShockIndicator` with new properties like:

```cpp
FLinearColor DotColor;
float PulseRateOverride;
```

---

## ✅ Best Practices

- Keep socket names consistent with the skeletal mesh you're targeting.
- Group related indicators close together in the builder for easy debugging.
- Don’t hardcode dot spawn logic — always use `Indicators` array.
- Use `nullptr` in `AddIndicator()` for optional image to avoid crashing.
- Remove or destroy all dot widgets when diagnosis changes.

---

## 🧱 Example Use Case: Cardiogenic Shock

```cpp
// In DiagnosisRegistery.cpp
DiagnosisBuilder(this)
  .SetName("Cardiogenic Shock")
  .AddIndicator("Wrist", "Pulse", "Faint wrist pulse detected", WristIcon)
  .AddIndicator("Face", "Pale Skin", "Skin tone loss near eyes", nullptr)
  .SetBehavior(NewObject<UCardiogenicShockBehavior>(this))
  .Build();
```

---
