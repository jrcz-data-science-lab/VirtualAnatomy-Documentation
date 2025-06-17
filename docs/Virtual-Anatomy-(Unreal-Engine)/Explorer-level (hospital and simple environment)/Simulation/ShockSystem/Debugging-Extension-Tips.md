# Debugging & Extension Tips for the Shock System

This section provides practical advice for debugging issues within the Shock System and offers guidelines for extending its functionality with new diagnoses, behaviors, and visual elements.

## Debugging Common Issues

### 1. Indicators (Breathing Dots / Temperature Labels) Not Appearing

* **Check `UCPP_Diagnosis` Configuration**: Ensure the `UCPP_Diagnosis` you are activating has its `Indicators` array populated in `UDiagnosisRegistery::BuildDiagnosisList()` via `DiagnosisBuilder::AddIndicator()`.
* **Verify Socket Names**: If `UShockIndicator::BoneName` or `UCPP_Diagnosis::CoreTempSocket`/`SkinTempSocket` is used, double-check that these socket names exactly match the sockets on your `TargetSkeletalMesh` (e.g., `BP_Bones`). Mismatched names are a common culprit.
* **UMG Widget Class**: In `UCPP_SimulationManager`, verify that `BreathingDotWidgetClass` (and any other UMG widget classes for indicators) is correctly assigned in the editor or during initialization.
* **Screen Position Calculation**: If indicators are UI widgets projected to screen, ensure `UShockIndicator::GetScreenPosition()` and the underlying Unreal Engine projection logic are working correctly, especially if the camera view changes.
* **`bIsRuntimeSpawned` Flag**: For `UBreathingDotWidget`, ensure `bIsRuntimeSpawned` is handled correctly if you have specific logic for runtime vs. editor-placed widgets.
* **`ClearActiveIndicators()`**: Confirm that `UCPP_SimulationManager::ClearActiveIndicators()` isn't being called prematurely, removing the widgets before they can be seen.

### 2. Niagara FX Not Spawning or Playing

* **`UCPP_Diagnosis` Configuration**: Ensure `NiagaraEffects` array in `UCPP_Diagnosis` is correctly populated via `DiagnosisBuilder::AddEffect()`.
* **`FDiagnosisEffect` Setup**: For each effect, verify:
    * `NiagaraSystem` is assigned to a valid `UNiagaraSystem` asset.
    * `bAttachToSocket` and `SocketName` are correctly set if attaching to a skeletal mesh.
    * `WorldLocation` is correct if spawning at a fixed point.
* **`ClearActiveEffects()`**: Ensure `UCPP_SimulationManager::ClearActiveEffects()` is not interfering with active effects.
* **Visibility/Activation**: Check if the Niagara components themselves are set to visible and auto-activate upon spawn. Use the Unreal Editor's World Outliner and Niagara component details panel during PIE (Play In Editor) to inspect.

### 3. Behavior Logic (e.g., Pulse Timer, Parameter Updates) Not Working

* **`UShockBehavior` Overrides**: Confirm that your specific `UShockBehavior` subclass (e.g., `UCardiogenicShockBehavior`) has correctly overridden `OnEnter()`, `OnExit()`, and `OnUpdate()` methods.
* **`CurrentShockBehavior` Assignment**: In `UCPP_SimulationManager`, ensure `CurrentShockBehavior` is correctly assigned to an instance of your desired `UShockBehavior` subclass when `ChangeDiagnosis()` is called. This is typically done within `UDiagnosisRegistery::BuildDiagnosisList()` when defining the `UCPP_Diagnosis`.
* **Parameter Passing**: When `UCPP_SimulationManager::UpdateSimulation()` is called, verify that the `FSimulationSlideBarsParameters` are correctly passed to `CurrentShockBehavior->OnUpdate()`.
* **Timer Handles**: For timed events like `PulseTimerHandle` in `UCardiogenicShockBehavior`, ensure timers are being set, cleared, and invalidated correctly. Use `GEngine->AddOnScreenDebugMessage` or breakpoints to trace timer execution.
* **Component References**: If your behavior relies on specific skeletal meshes (e.g., `LungMesh` in `UObstructiveShockBehavior`) or dynamic material instances, ensure these are correctly acquired and stored during `OnEnter()`.

## Extension Tips

### 1. Adding a New Diagnosis

1.  **Define New `EDiagnosisType`**: Add a new entry to your `EDiagnosisType` enum.
2.  **Create New `UShockBehavior` Subclass**: Create a new C++ class that inherits from `UShockBehavior` (e.g., `UNewShockBehavior`).
3.  **Implement `OnEnter`, `OnExit`, `OnUpdate`**: Override these virtual functions in your new behavior class to define the unique runtime logic for this diagnosis. This includes:
    * Spawning specific Niagara FX using `SpawnNiagaraFX()` (similar to `UCardiogenicShockBehavior`).
    * Spawning `AFloatingTemperatureLabel` actors using `SpawnTemperatureLabels()` (if applicable).
    * Setting up any unique timers or material changes.
    * Responding to `FSimulationSlideBarsParameters` changes in `OnUpdate()`.
4.  **Register in `UDiagnosisRegistery`**: In `UDiagnosisRegistery::BuildDiagnosisList()`, use `DiagnosisBuilder` to define your new `UCPP_Diagnosis`. Crucially, instantiate your new `UNewShockBehavior` and assign it to the `UCPP_Diagnosis`'s `RuntimeBehavior` property.
    ```cpp
    // Example in UDiagnosisRegistery::BuildDiagnosisList()
    DiagnosisBuilder(Outer)
        .SetName("New Custom Shock")
        .SetDescription("Description for new shock.")
        .SetDiagnosisType(EDiagnosisType::NewCustomShock) // Your new enum value
        .SetSimulationParameters(FSimulationSlideBarsParameters(1.0f, 120.f, 0.5f))
        .AddEffect(NewNiagaraSystemAsset, true, FName("NewSocket"))
        .AddIndicator(FName("AnotherSocket"), FText::FromString("New Indicator"), FText::FromString("Detail for new indicator"), nullptr)
        .SetBehavior(NewObject<UNewShockBehavior>(Outer)) // Instantiate your new behavior
        .Build();
    ```
5.  **Update UI (if necessary)**: If your UI has a dropdown for diagnoses, ensure your new `EDiagnosisType` is reflected.

### 2. Adding a New Visual Indicator Type (Beyond Breathing Dots)

If you need a completely new type of UI indicator (e.g., a flashing warning icon, a dynamic graph):

1.  **Create New UMG Widget**: Create a new `UUserWidget` subclass (e.g., `UWarningIconWidget`).
2.  **Create New `UShockIndicator` Subclass (Optional but Recommended)**: If the new indicator has distinct properties or complex logic for its 3D-to-2D projection or interaction, consider creating a new `UShockIndicator` subclass (e.g., `UWarningShockIndicator`). This keeps properties and specific logic encapsulated.
3.  **Modify `UCPP_SimulationManager::SpawnIndicators`**: You would need to extend `UCPP_SimulationManager::SpawnIndicators` to recognize your new `UShockIndicator` subclass (if you created one) or to simply instantiate your new UMG widget based on some identifier in the `UCPP_Diagnosis`. A more flexible approach might involve having a `TSubclassOf<UUserWidget>` property on `UShockIndicator` itself.

### 3. Creating Custom Niagara Effects

1.  **Design in Niagara Editor**: Create your new `UNiagaraSystem` assets in the Unreal Engine Niagara Editor.
2.  **Add to `UCPP_Diagnosis`**: Reference these new Niagara System assets in the `NiagaraEffects` array of your `UCPP_Diagnosis` definitions (via `DiagnosisBuilder::AddEffect()`).

---