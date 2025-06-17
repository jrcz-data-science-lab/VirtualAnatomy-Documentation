# Breathing Dots

The "Breathing Dots" are a key visual feedback mechanism within the Shock System, providing immediate and intuitive information about physiological states or interactive points on the anatomical model. (All the elements used for it can be found inside Unreal Engine a folder called ButtonsForShocks)

## Concept and Purpose

These dots are dynamic UI elements that visually change (e.g., color, size, opacity, animation) to signify:
* **Vital Signs:** Representing a pulse, respiration rate, or other continuous physiological data in a visually engaging manner.
* **Interactive Hotspots:** Guiding the user to specific areas that can be interacted with to trigger events or apply medical interventions.
* **Status Indicators:** Showing the current "health" or state of a particular organ or system within a specific diagnosis.

## Implementation Details

Breathing Dots are managed by the `UCPP_SimulationManager` and defined within each `UCPP_Diagnosis` profile.

### Core Classes: `UShockIndicator` and `UBreathingDotWidget`

1.  **`UShockIndicator` (Base Class)**:
    * This is the base C++ class (`Simulation/Diagnosis/Indicators/ShockIndicator.h`) that conceptually represents a "shock indicator." It holds common data like the `BoneName` it should attach to, and potentially the `Title`, `Description`, and `Image` used for a tooltip or pop-up associated with the dot.
    * It is a `UObject` and managed by the `UCPP_Diagnosis` to define the *properties* of an indicator.
    * It includes a `GetScreenPosition` method to project the 3D bone location to a 2D screen position.
    * It also contains a private `DotWidgetRef` which is set when the widget is spawned by the `SimulationManager`.

2.  **`UBreathingDotWidget` (UMG Widget)**:
    * Defined in `BreathingDotWidget.h`. This is a `UUserWidget` subclass, meaning it's a Blueprint-implementable UI widget.
    * It contains a `UPROPERTY(meta = (BindWidgetAnim), Transient) UWidgetAnimation* ImageBreathing;` which is a crucial part. This property is designed to be linked to an animation created in the Unreal Engine UMG editor (e.g., in `WBP_BreathingDotWidget`). This animation is what gives the "breathing" pulse effect.
    * The `bIsRuntimeSpawned` flag helps distinguish widgets spawned by the simulation from those placed manually in the editor.
    * Its `NativeConstruct()` method is where you would typically start the `ImageBreathing` animation or hide editor-only debugging UI.

### Integration with `UCPP_Diagnosis` and `UCPP_SimulationManager`

* **Defining in `UCPP_Diagnosis`**: Each `UCPP_Diagnosis` object holds an array of `UShockIndicator*` (`UPROPERTY() TArray<UShockIndicator*> Indicators;`). This array is populated using the `AddIndicator()` method of the `DiagnosisBuilder`.
    ```cpp
    // Example from DiagnosisBuilder (conceptual)
    // AddIndicator(FName BoneName, const FText& Title, const FText& Description, UTexture2D* Image);
    DiagnosisBuilder(this)
        .SetName("Cardiogenic Shock")
        // ... other settings
        .AddIndicator(FName("HeartSocket"), FText::FromString("Cardiac Pulse"), FText::FromString("Monitors heart rate and rhythm."), HeartIconTexture)
        .Build();
    ```
* **Spawning by `UCPP_SimulationManager`**: When a diagnosis is changed or started, the `UCPP_SimulationManager` calls `SpawnIndicators(const UCPP_Diagnosis& Diagnosis)`. This function is responsible for:
    1.  Iterating through the `Indicators` array of the active `UCPP_Diagnosis`.
    2.  For each `UShockIndicator`, it creates an instance of `UBreathingDotWidgetClass` (a `TSubclassOf<UBreathingDotWidget>` exposed in `UCPP_SimulationManager`'s properties).
    3.  It then attaches these `UBreathingDotWidget` instances to the `TargetSkeletalMesh` (e.g., `BP_Bones`) at the specified `BoneName` (socket) from the `UShockIndicator`, likely converting the 3D socket location to a 2D screen position for the UMG widget.
    4.  The spawned widgets are added to the `ActiveIndicators` array in `UCPP_SimulationManager` for later management (e.g., `ClearActiveIndicators()`).

## Customization and Best Practices

* **Blueprint Customization**: `UBreathingDotWidget` is designed to be extended in Blueprint. You can customize its visual appearance, add more complex animations, and implement interactive logic directly within the Blueprint subclass (`WBP_BreathingDotWidget`).
* **Material Parameters**: The "breathing" effect itself is often driven by a Material Instance. You can expose parameters in your material (e.g., `EmissiveColor`, `PulseSpeed`) that can be dynamically updated by C++ or Blueprint logic to change the dot's visual behavior based on real-time simulation data.
* **Performance**: Be mindful of the number of active breathing dots, especially if they involve complex animations or many materials, to maintain good performance.

---

For how these indicators fit into the broader system, see [[How Everything Connects at Runtime]].