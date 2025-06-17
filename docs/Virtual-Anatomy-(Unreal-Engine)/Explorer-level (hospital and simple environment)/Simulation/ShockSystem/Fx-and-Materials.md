# FX and Materials

Visual effects (FX) and dynamically controlled materials are critical for enhancing the realism and clarity of the Shock System. They provide immediate visual cues for physiological states, simulated interventions, and environmental changes. ( They can be found inside Unreal Engine a folder called Niagara.)

## Visual Effects (FX) - Niagara Systems

The Shock System primarily utilizes Unreal Engine's **Niagara** particle system for complex visual effects. These effects are defined within each `UCPP_Diagnosis` and are managed by the `UCPP_SimulationManager`.

### `FDiagnosisEffect` Structure

The `FDiagnosisEffect` struct (`DiagnosisEffect.h`) is used to define a single Niagara FX that should be spawned for a given diagnosis. It contains:

* **`UNiagaraSystem* NiagaraSystem`**: A reference to the actual Niagara particle system asset (e.g., `NS_Sweat`, `NS_PulseEffect`).
* **`bool bAttachToSocket`**: If `true`, the effect will be attached to a specific bone or socket on the `TargetSkeletalMesh`.
* **`FName SocketName`**: The name of the socket if `bAttachToSocket` is true.
* **`FVector WorldLocation`**: The world coordinates to spawn the effect if `bAttachToSocket` is `false`.

### Integration with `UCPP_Diagnosis` and `UCPP_SimulationManager`

* **Defining in `UCPP_Diagnosis`**: Each `UCPP_Diagnosis` object stores an array of `FDiagnosisEffect` (`UPROPERTY() TArray<FDiagnosisEffect> NiagaraEffects;`). These effects are added using the `AddEffect()` method of the `UCPP_Diagnosis` class, typically called via the `DiagnosisBuilder`.
    ```cpp
    // Example from DiagnosisBuilder (conceptual, DiagnosisBuilder calls AddEffect internally)
    DiagnosisBuilder(this)
        .SetName("Septic Shock")
        // ... other settings
        .AddEffect(NS_SweatEffect, true, FName("ForeheadSocket")) // Attach sweat to forehead
        .AddEffect(NS_BloodDilutionEffect, false, FVector(0,0,0)) // World-space effect
        .Build();
    ```
* **Spawning and Clearing by `UCPP_SimulationManager`**:
    * When a diagnosis is activated (via `ChangeDiagnosis()` or `StartSimulation()`), the `UCPP_SimulationManager` iterates through the `NiagaraEffects` array of the active `UCPP_Diagnosis`.
    * For each `FDiagnosisEffect`, it spawns a `UNiagaraComponent` instance and either attaches it to the `TargetSkeletalMesh` (at `SocketName`) or places it at `WorldLocation`.
    * All active Niagara components are stored in `UCPP_SimulationManager`'s `ActiveNiagaraComponents` array.
    * When a diagnosis changes or stops, `UCPP_SimulationManager::ClearActiveEffects()` is called to destroy all previously spawned Niagara components, ensuring a clean state.

## Materials

While not explicitly managed by dedicated classes like `FDiagnosisEffect`, materials play a crucial role in the visual representation of shock.

* **Dynamic Material Instances**: Many visual changes (e.g., skin pallor, redness, distortion, "breathing" pulse for dots) are achieved by creating **Dynamic Material Instances (DMIs)** at runtime. For example, `UObstructiveShockBehavior` directly manipulates a `UMaterialInstanceDynamic` (`LungMaterialInstance`) to tint the lung mesh.
* **Parameter Control**: C++ or Blueprint code can then set parameters on these DMIs (e.g., `SetVectorParameterValue`, `SetFloatParameterValue`) to change colors, opacity, emission, or distortion effects in real-time based on simulation data.
* **Post-Processing Materials**: Global visual cues (e.g., screen-wide desaturation, blur, or a subtle overlay effect) can be applied using Post-Process Materials, which are also often driven by simulation parameters.

## Floating Temperature Labels (`AFloatingTemperatureLabel`)

The `AFloatingTemperatureLabel` actor (`FloatingTemperatureLabel.h`) is a specialized visual indicator that displays core and skin temperatures in 3D space.

* **Purpose**: Provides immediate numerical feedback on critical physiological temperatures.
* **Implementation**:
    * It's an `AActor` with a `UTextRenderComponent` that renders text in the world.
    * `InitializeLabel()` sets the initial text and color.
    * `UpdateLabel()` can be called at runtime by `UCPP_SimulationManager::UpdateTemperatureDisplays()` to reflect changing temperatures.
* **Configuration in `UCPP_Diagnosis`**: Each `UCPP_Diagnosis` defines whether temperature labels should be shown (`bShowTemperature`), their base values (`BaseCoreTemperature`, `BaseSkinTemperature`), the sockets they attach to (`CoreTempSocket`, `SkinTempSocket`), and their text colors.
* **Spawning**: The `UCPP_Diagnosis::SpawnTemperatureLabels()` method is responsible for spawning these actors and attaching them to the skeletal mesh at the specified sockets. `UCPP_SimulationManager` manages the lifetime and updates of these labels.

---

For how these FX and Material systems are integrated into the overall simulation flow, refer to [[How Everything Connects at Runtime]].