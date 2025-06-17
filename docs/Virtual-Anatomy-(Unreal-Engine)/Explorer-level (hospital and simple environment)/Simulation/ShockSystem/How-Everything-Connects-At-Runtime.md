# How Everything Connects at Runtime

Understanding the runtime flow is crucial for comprehending how the Shock System integrates all its components to simulate a medical diagnosis. The `UCPP_SimulationManager` acts as the central orchestrator, delegating tasks and managing the lifecycle of various elements.

## Key Class Interactions

Let's trace the typical sequence of events and interactions between the core classes:

### 1. Initialization and Diagnosis Registration

* **`UCPP_SimulationManager::BeginPlay()`**:
    * Creates an instance of `UDiagnosisRegistery`.
    * Calls `DiagnosisRegistery->Initialize(this)`, passing itself as the `Outer` for proper UObject ownership.
* **`UDiagnosisRegistery::Initialize(UObject* InOuter)`**:
    * Sets its internal `Outer` reference.
    * Calls `BuildDiagnosisList()`.
* **`UDiagnosisRegistery::BuildDiagnosisList()`**:
    * This is where all predefined `UCPP_Diagnosis` objects are constructed using the `DiagnosisBuilder`.
    * For each diagnosis (e.g., Cardiogenic Shock, Septic Shock), a `DiagnosisBuilder` instance is used to define its:
        * `Name` and `Description`
        * `EDiagnosisType`
        * `FSimulationSlideBarsParameters` (default values for speed, BPM, etc.)
        * `FBloodPressure`
        * `FDiagnosisEffect` (Niagara FX assets to spawn) via `AddEffect()`
        * `UShockIndicator` (Breathing Dot definitions) via `AddIndicator()`
        * Temperature display settings via `SetTemperatureDisplay()`
        * **Crucially**: It also instantiates and sets the `UShockBehavior* RuntimeBehavior` for each diagnosis (e.g., `NewObject<UCardiogenicShockBehavior>(Outer)`), linking the diagnosis data to its specific runtime logic.
    * The fully configured `UCPP_Diagnosis*` objects are stored in `DiagnosisList`.

### 2. Changing a Diagnosis (`UCPP_SimulationManager::ChangeDiagnosis`)

This is the primary entry point for activating a specific medical scenario.

1.  **`UCPP_SimulationManager::ChangeDiagnosis(EDiagnosisType NewDiagnosisType)`**:
    * **Clear Previous State**: If a `CurrentShockBehavior` exists, its `OnExit()` method is called to clean up any diagnosis-specific elements. All active indicators and Niagara effects are cleared via `ClearActiveIndicators()` and `ClearActiveEffects()`.
    * **Retrieve New Diagnosis**: Fetches the `UCPP_Diagnosis` object corresponding to `NewDiagnosisType` from the `DiagnosisRegistery` using `GetDiagnosisByType()`.
    * **Activate New Behavior**: The `RuntimeBehavior` (`UShockBehavior*`) from the selected `UCPP_Diagnosis` is set as the `CurrentShockBehavior` in the `UCPP_SimulationManager`.
    * **Call `OnEnter`**: The `CurrentShockBehavior->OnEnter(ActiveDiagnosis, this)` method is called. This is where diagnosis-specific setup occurs.
        * For example, `UCardiogenicShockBehavior::OnEnter` might:
            * Call `SpawnNiagaraFX()` to activate specific particle effects.
            * Call `SpawnTemperatureLabels()` to display 3D temperature readings.
            * Initiate `StartPulseTimer()` to control BPM-driven pulse effects.
    * **Spawn Indicators**: `UCPP_SimulationManager::SpawnIndicators()` is called, which iterates through `ActiveDiagnosis->Indicators` and creates `UBreathingDotWidget` instances, positioning them relative to the `TargetSkeletalMesh` sockets.
    * **Update UI**: The `FChangeDiagnosis` delegate is broadcast, allowing UI elements (like the diagnosis dropdown menu) to update their display.

### 3. Updating Simulation Parameters (`UCPP_SimulationManager::UpdateSimulation`)

This handles real-time changes to simulation parameters (e.g., via UI sliders for BPM, speed).

1.  **`UCPP_SimulationManager::UpdateSimulation(FSimulationSlideBarsParameters* Params)`**:
    * **Update Current Parameters**: The `SimulationParameters` within `UCPP_SimulationManager` are updated with the new `Params`.
    * **Call `OnUpdate`**: `CurrentShockBehavior->OnUpdate(Params)` is called. This allows individual shock behaviors to react to parameter changes.
        * For example, `UCardiogenicShockBehavior::OnUpdate` might:
            * Adjust the interval of its `PulseTimerHandle` based on the new BPM value via `GetPulseInterval()`.
            * Update `AFloatingTemperatureLabel` texts if temperatures are dynamic and linked to parameters.
    * **Broadcast Update**: The `FUpdateSimulation` delegate is broadcast, enabling other systems to react to parameter changes.

### 4. Stopping the Simulation (`UCPP_SimulationManager::StopSimulation`)

1.  **`UCPP_SimulationManager::StopSimulation()`**:
    * **Call `OnExit`**: `CurrentShockBehavior->OnExit()` is called for final cleanup.
    * **Clear All Visuals**: `ClearActiveIndicators()` and `ClearActiveEffects()` are called to remove all spawned UI dots, Niagara FX, and temperature labels from the scene.
    * **Broadcast Stop**: The `FStopSimulation` delegate is broadcast.

## Flow Diagram

```mermaid
graph TD
    A[BeginPlay (UCPP_SimulationManager)] --> B{Create UDiagnosisRegistery};
    B --> C{Initialize UDiagnosisRegistery (Build DiagnosisList)};
    C --> D[UCPP_SimulationManager Ready];

    D -- ChangeDiagnosis(NewType) --> E{Clear Previous State};
    E --> F{Get UCPP_Diagnosis by Type};
    F --> G{Set CurrentShockBehavior};
    G --> H{Call CurrentShockBehavior->OnEnter()};
    H --> I{SpawnIndicators()};
    I --> J{UpdateTemperatureDisplays()};
    J --> K{Broadcast FChangeDiagnosis};
    K --> L[Simulation Running];

    L -- UpdateSimulation(NewParams) --> M{Update SimulationParameters};
    M --> N{Call CurrentShockBehavior->OnUpdate()};
    N --> O{Broadcast FUpdateSimulation};
    O --> L;

    L -- StopSimulation() --> P{Call CurrentShockBehavior->OnExit()};
    P --> Q{ClearAllVisuals()};
    Q --> R{Broadcast FStopSimulation};
    R --> S[Simulation Stopped];