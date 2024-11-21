# Simulation button


## Public Methods 

### `void OnSimulationButtonClick()`

Function that is called when the user presses the button to toggle the simulation.

---

### `bool GetIsSimulationRunning()`

**Returns**:

- `bool` - `true` if the simulation is running; `false` otherwise.

Gets the current state of the simulation. This is mainly for checks done by widgets instantiating this class.

---

## Protected Methods 

### `void NativePreConstruct()`

Overrides the base class method to implement pre-construction logic for the widget.

---

### `void SynchronizeProperties()`

Overrides the base class method to synchronize properties before the widget is displayed.

---

### `void NativeConstruct()`

Overrides the base class method to implement logic that runs when the widget is constructed.

---

### `void NativeDestruct()`

Overrides the base class method to implement cleanup logic when the widget is destroyed.

---

## Private Methods 

### `FSlateBrush GetCorrectButtonImageStyle(bool simulationState)`

**Parameters**:

- `bool simulationState` - The current state of the simulation. `true` if the simulation is running, `false` otherwise.

Configures the style of the button based on the state of the simulation.

---

### `FButtonStyle GetCorrectButtonStyle(bool simulationState)`

**Parameters**:

- `bool simulationState` - The current state of the simulation. `true` if the simulation is running, `false` otherwise.

Configures the icon of the button based on the simulation state and returns a brush representing the action that can be taken.

---

### `void SetButtonStylings(bool simulationState)`

**Parameters**:

- `bool simulationState` - The current state of the simulation. `true` if the simulation is running, `false` otherwise.

Sets the color and texture of the button along with the desired image size. This method calls `GetCorrectButtonStyle` and `GetCorrectButtonImageStyle` to apply the appropriate styles.
