# `UCPP_SlicerSideBar`

A user interface widget class for controlling the slicer in the Virtual Anatomy Unreal Engine project.

This class is responsible for:

- Displaying/hiding the slicer plane
- Handling checkbox interaction for enabling/disabling the slicer
- Modifying distance and rotation of the slicer plane
- Providing buttons for quick access to predefined values
- Managing error states when slicer components are not found
- Interacting with the slicer actor in the 3D world

## Public Methods

### `void SetSlicerActor(ACPP_Slicer* InSlicerActor)`

**Description:**

Sets the slicer actor.

**Parameters:**

- `InSlicerActor` – the slicer actor to associate with the sidebar.

### `void ShowSlicerPlane()`

**Description:**

Shows the slicer plane. Called when the sidebar is shown.

### `void HideSlicerPlane()`

**Description:**

Hides the slicer plane. Called when the sidebar is hidden.

### `bool GetSlicerErrorState()`

**Description:**

Checks whether the slicer is giving an error.

**Returns:**

- `true` if there's an error; `false` otherwise.

### `hangeSlicerCheckbox(bool bIsChecked)`

**Description:**

Handles checkbox state change.

**Parameters:**

- `bIsChecked` – `true` if checked; `false` otherwise.

### `ChangeSlicerDistance(float NewValue)`

**Description:**

Updates the slicer distance.

**Parameters:**

- `NewValue` – new distance value from slider or button.

### `ChangeSlicerRotation(float NewValue)`

**Description:**

Updates the slicer rotation.

**Parameters:**

- `NewValue` – new rotation value from slider or button.

## Protected Properties

- **`UCPP_GameInstance* GameInstance`** – Reference to the game instance to load saved slicer parameters.
- **`ACPP_Slicer* SlicerActor`** – Reference to the slicer actor in the scene.
- **`bool bIsSlicerGivingError`** – Set to `true` if slicer fails to initialize correctly.
- **`FSlicerSlideBarsParameters SlicerParameters`** – Struct holding the distance and rotation slider values.

## Predefined Button Click Handlers

Functions triggered when clicking buttons with fixed distance or rotation values.

### Distance button handlers

- `void OnDistance0Click()` – Sets distance to 0
- `void OnDistance50Click()` – Sets distance to 50
- `void OnDistance100Click()` – Sets distance to 100
- `void OnDistance150Click()` – Sets distance to 150
- `void OnDistance200Click()` – Sets distance to 200

### Rotation button handlers

- `void OnRotationRearClick()` – Sets rotation to 90°
- `void OnRotationFrontClick()` – Sets rotation to 270°
- `void OnRotationLeftClick()` – Sets rotation to 0°
- `void OnRotationRightClick()` – Sets rotation to 180°

## UI Widgets

Widgets used in the sidebar (bound via UMG):

- `UCPP_SlicerSlideBar* DistanceSlider`: Controls distance of the slicer
- `UCPP_SlicerSlideBar* RotationSlider`: Controls rotation of the slicer
- `UCheckBox* SlicerCheckbox`: Checkbox for enabling/disabling slicer

### Predefined Buttons (UButton*)

- Distance: `Distance0`, `Distance50`, `Distance100`, `Distance150`, `Distance200`
- Rotation: `RotationRear`, `RotationFront`, `RotationLeft`, `RotationRight`