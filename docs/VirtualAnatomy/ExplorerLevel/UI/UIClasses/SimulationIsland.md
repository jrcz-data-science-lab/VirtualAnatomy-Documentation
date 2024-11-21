## Class Description

**UCPP_SimulationIsland**: 

Simulation island that displays details about the ongoing simulation.

---

## Public Properties 

### `TObjectPtr<UCPP_SimulationButton> SimulationStartButton`

**Description**: 

A button that starts the simulation. This property is bindable to the widget.

---

### `TObjectPtr<UWidgetAnimation> SimulationIslandAnimation`

**Description**: 

A widget animation used to hide or show the simulation island. This property is bindable to the widget and is marked as transient.

---

## Public Methods 

### `void OnStartSimulation()`

C++ implementation of the function that is executed when the user presses the start simulation button.

---

### `void OnStopSimulation()`

C++ implementation of the function that is executed when the user presses the stop simulation button.

---

## Protected Methods 

### `void NativeConstruct()`

Overrides the base class method to implement logic that runs when the widget is constructed.

---

### `void NativeDestruct()`

Overrides the base class method to implement cleanup logic when the widget is destroyed.

---

## Private Properties 

### `TObjectPtr<UCPP_SimulationManager> M_SimulationManager`

**Description**: 

An instance of the simulation manager class, used to manage the simulation's state.
