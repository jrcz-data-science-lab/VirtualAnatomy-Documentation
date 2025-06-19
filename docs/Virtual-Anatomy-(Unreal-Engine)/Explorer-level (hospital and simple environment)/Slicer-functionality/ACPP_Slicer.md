# ACPP_Slicer

This class is responsible for controlling and visualizing a slicing plane in the application. It manages the position, rotation, and visibility of the slicer, synchronizing these properties with a material parameter collection used for mesh slicing.

## Public methods

### `ACPP_Slicer()`

**Description**:
Constructor for the `ACPP_Slicer` class.

### `virtual void Tick(float DeltaTime)`

**Description**:  
Called every frame. Currently unused, but available for runtime behavior changes.

**Parameters**:
- `DeltaTime`: Time elapsed since the last frame.

### `virtual void SetupPlayerInputComponent(UInputComponent* PlayerInputComponent)`

**Description**:  
Binds input actions to the pawn. Not used in this slicer as control is typically handled via UI.

**Parameters**:
- `PlayerInputComponent`: The input component used for binding controls.

### `void SetRotation(const FRotator& Rotation)`

**Description**:  
Sets the rotation of the slicer by rotating the attached `SpringArmComponent`.

**Parameters**:
- `Rotation`: New world-space rotation to apply to the slicer arm.

### `FRotator GetRotation()`

**Description**:  
Retrieves the last stored rotation of the slicer. This value reflects the initial rotation, not live updates from the component.

**Returns**:
- `FRotator`: Last known slicer rotation.

### `void SetSpringArmLength(float Length)`

**Description**:  
Changes the length of the spring arm to move the slicer plane further or closer to the origin. Updates the material parameters on the next tick.

**Parameters**:
- `Length`: The new distance (in cm) from the pivot point.

### `UStaticMeshComponent* GetSlicerPlane()`

**Description**:  
Returns the static mesh component used as the slicer plane.

**Returns**:
- `UStaticMeshComponent*`: The slicing plane component.

### `void ToggleSlicer(float NewValue) const`

**Description**:  
Toggles the slicer's visibility or activity by setting a scalar value in the material collection (where 1 is off, 0 = on !!!).

**Parameters**:
- `NewValue`: Float value used to toggle state. `0.0` = on, `1.0` = off.

### `void UpdateSlicerProperties() const`

**Description**:  
Updates the slicer's location and orientation in the associated material parameter collection and thus updates its 'slice' visualization in the scene.

## Protected methods

### `virtual void BeginPlay()`

**Description**:  
Initializes references to components, retrieves stored slicer values from the game instance, and prepares the slicer's visual effect.

### `virtual void EndPlay(const EEndPlayReason::Type EndPlayReason)`

**Description**:  
Saves current slicer values (position, rotation, active state) into the game instance when the actor is removed from the world.

**Parameters**:
- `EndPlayReason`: Enum value describing why the actor is ending play.

## Protected properties

### `UStaticMeshComponent* PlaneComponent`

**Description**:  
The visible slicing plane. Used for both visualization and defining the slicing region in materials.

### `USpringArmComponent* SpringArmComponent`

**Description**:  
Controls the distance and rotation of the slicer relative to the slicer's origin. Enables intuitive adjustments to the slicer position.

### `UMaterialParameterCollectionInstance* MPC_Instance`

**Description**:  
Represents the runtime instance of the material parameter collection. Used to dynamically set slicer-related parameters (`Position`, `Direction`, `isSlicerOff`).

### `FRotator Rotation`

**Description**:
Stores the last known rotation of the slicer. Used to maintain state across frames and ensure consistent behavior.

### `UCPP_GameInstance* GameInstance`

**Description**:
Reference to the custom game instance used for storing and retrieving slicer state across sessions.

## Notes

- If `PlaneComponent` or/and `SpringArmComponent` are missing, the slicer destroys itself, preventing further functionality and possible crashes.
- If the material parameter collection or its instance cannot be found, the slicer will also destroy itself to avoid the possible crashes.
- Uses `GetWorld()->GetTimerManager().SetTimerForNextTick(...)` to ensure material properties update correctly after changing length.
