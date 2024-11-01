# UCPP_BaseAnimation Class

This class serves as the base class for all potential animations in the game. It is designed to be inherited by other C++ classes, not directly by Blueprints. Its primary purpose is to start or stop animations in response to events fired by the `SimulationManager` class. This class may also be extended to hold various parameters in the future.

## Protected Properties

### `TObjectPtr<UCPP_SimulationManager> SimulationManager`

**Description**: 

An instance of the `SimulationManager`, allowing the animation to listen for events triggered by it.

---

### `TObjectPtr<UAnimSequence> AnimationOfHumanBody`

**Description**: 

The animation sequence that will play when the simulation starts.

---

## Protected Methods

### `void OnStartSimulation()`

**Description**: 

Executed when the simulation start event is triggered by the `SimulationManager`.

---

### `void OnStopSimulation()`

**Description**: 

Executed when the simulation end event is triggered by the `SimulationManager`.

---

## Public Methods

### `void NativeBeginPlay()`

**Description**: 

An overridden method that subscribes the animation instance to events from the `SimulationManager`.

---

### `void BeginDestroy()`

**Description**: 

An overridden method for any cleanup needed when the animation instance is destroyed.

