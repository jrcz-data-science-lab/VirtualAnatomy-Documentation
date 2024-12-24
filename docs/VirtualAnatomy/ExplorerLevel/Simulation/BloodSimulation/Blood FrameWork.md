# Blood framework

# Blood Particle System Framework

## Overview

There are multiple actors representing blood in the simulation, such as:
- Blood squirting from the body during hypervolemic shock.
- Blood particles following specific paths.

All these actors are connected to the **Simulation Manager** and must listen to various events dispatched by it.

To enforce the **DRY (Don't Repeat Yourself)** principle, a small framework was created. This framework uses inheritance to define a base class for all blood-related actors, called `ACPP_BloodParticleSystemBase`. This base class is then used as the foundation for derived classes, such as:

- `CPP_BloodPathSystem`

- `CPP_RupturedArtery`

---

## Specifications of `ACPP_BloodParticleSystemBase`

The base class provides:
- A unified interface for handling simulation events related to blood particle systems.

- Simplified management of parameters and properties for blood-related actors.

- Easy integration with the simulation manager for event handling.

Derived classes like `CPP_BloodPathSystem` and `CPP_RupturedArtery` inherit the functionalities of the base class, reducing redundancy and promoting clean, maintainable code.

## Class Description

**ACPP_BloodParticleSystemBase**: 

This is a base class designed for handling blood-related particle systems in the Unreal Engine. It provides virtual methods for interaction with the simulation manager, ensuring that inherited classes can seamlessly integrate with simulation functionalities. The class simplifies the management and propagation of blood system parameters, allowing global changes to be reflected across all associated particle systems.

### Key Features:
- Provides a framework for interacting with the simulation manager.
- Simplifies parameter updates across multiple blood-related components.
- Encourages inheritance for custom blood particle system implementations.

---

## Public Methods

### `ACPP_BloodParticleSystemBase()`

**Description**:  
The default constructor that sets initial values for the actor's properties.  

---

### `void Init(UCPP_SimulationManager* SimManager, UNiagaraSystem* Blood)`

**Description**:  
Initializes the blood particle system with a reference to the simulation manager and a Niagara system for blood particles.  

**Parameters**:  
- `SimManager`: A pointer to the simulation manager that manages the simulation logic.  
- `Blood`: A pointer to the Niagara system representing blood particles.  

---

### `UFUNCTION(BlueprintCallable, Category = "Simulation|BloodFlow") void Init(UCPP_SimulationManager* SimManager)`

**Description**:  
A Blueprint-callable method to initialize the particle system with a reference to the simulation manager.  

**Parameters**:  
- `SimManager`: A pointer to the simulation manager.  

---

### `virtual void Tick(float DeltaTime)`

**Description**:  
Called every frame to update the actor.  

**Parameters**:  
- `DeltaTime`: The time elapsed since the last frame.  

---

## Protected Methods

### `virtual void BeginPlay()`

**Description**:  
Called when the game starts or the actor is spawned.  

---

### `virtual void HandleSimulationStart()`

**Description**:  
Handles logic to execute when the simulation starts. This method is intended to be overridden in derived classes.  

---

### `virtual void HandleSimulationEnd()`

**Description**:  
Handles logic to execute when the simulation ends. This method is intended to be overridden in derived classes.  

---

### `virtual void HandleSimulationUpdate(FSimulationSlideBarsParameters* UpdatedParameters)`

**Description**:  
Handles updates to the simulation parameters.  

**Parameters**:  
- `UpdatedParameters`: A pointer to the updated simulation parameters.  

---

### `virtual void HandleDiagnosisChagne(UCPP_Diagnosis& selectedDiagnosis)`

**Description**:  
Handles changes to the selected diagnosis.  

**Parameters**:  
- `selectedDiagnosis`: A reference to the updated diagnosis.  

---

## Properties

### Protected Properties

#### `USceneComponent* Root`

**Description**:  
The root scene component for the actor.  

**Access Modifier**: `VisibleAnywhere`  

---

#### `UNiagaraComponent* BloodParticleComponent`

**Description**:  
The Niagara component responsible for handling blood particle effects.  

**Access Modifier**: `VisibleAnywhere`  

---

#### `UNiagaraSystem* BloodNiagaraSystem`

**Description**:  
The Niagara system used to define the blood particle effects.  

**Access Modifier**: `EditAnywhere`  

---

#### `UCPP_SimulationManager* SimulationManager`

**Description**:  
A pointer to the simulation manager that oversees the simulation logic.  

---

## Notes

- **Inheritance Requirement**:  
  Classes inheriting from `ACPP_BloodParticleSystemBase` must include Unreal Engine-specific macros (`GENERATED_BODY()`, `UCLASS`, etc.).  
- This class provides a centralized mechanism for managing particle systems related to blood, making it highly reusable and maintainable.  
- Ensure proper initialization by calling `Init()` before using the particle system.  

---

## Usage

To use this class:  
1. Inherit from `ACPP_BloodParticleSystemBase` for any custom blood particle system implementation.  
2. Override the virtual methods (`HandleSimulationStart`, `HandleSimulationEnd`, etc.) to define specific behaviors.  
3. Initialize the system using the `Init()` method, providing references to the simulation manager and the Niagara system.
