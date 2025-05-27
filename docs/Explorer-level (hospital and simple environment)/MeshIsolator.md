# Isolate Mesh

## Class Description

**`UCPP_IsolateMesh`:**  
This class is responsible for isolating a selected part of a 3D model by repositioning both the camera's focus point and the chosen mesh to another plane within the explorer level. The selection is handled via the `MeshSelector` class.

---

## Public Methods

### `UCPP_IsolateMesh()`

**Description:**  
Constructor that sets default values for the class.

---

### `void Init(UWorld* World)`

**Description:**  
Initializes an instance of this class within the given world.  
- **Parameters:**  
  - `World`: A pointer to the `UWorld` instance where the class is initialized.

---

### `void MoveCenterPoint()`

**Description:**  
Handles the transformation of the camera's target focus point and the selected mesh. This function is triggered upon user interaction by clicking a button.

---

## Public Properties

### `TObjectPtr<ACPP_User> m_userRef`

**Description:**  
A reference to an `ACPP_User` object, primarily used to retrieve the `TargetActor`, which is the focus point of the camera.  
- **Category:** MeshIsolator  
- **Attributes:** `BlueprintReadWrite`, `EditAnywhere`

---

## Private Properties

### `TObjectPtr<UWorld> m_world`

**Description:**  
A pointer to the `UWorld` instance associated with this class. This is used for accessing the game world and its components.

---

### `TObjectPtr<UCPP_CameraControls> m_camera`

**Description:**  
A reference to the `UCPP_CameraControls` instance. This is utilized for methods that manipulate the camera's focus point.

---

### `TObjectPtr<UMeshSelector> m_selector`

**Description:**  
A pointer to the `MeshSelector` class, which provides access to the currently selected mesh by the user.

---

### `bool HasCenterPointMoved`

**Description:**  
A boolean property that tracks whether the center point (camera focus) has been adjusted.  
- **Attributes:** `UPROPERTY()`

---

### `FVector targetActorLocation`

**Description:**  
Stores the initial location of the `TargetActor`, representing the default camera focus point before any transformations.

---

### `FVector meshOriginalLocation`

**Description:**  
Holds the original position of the selected mesh before being relocated.
