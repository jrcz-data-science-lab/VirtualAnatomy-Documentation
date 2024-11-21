# Camera Controls Documentation

## Class Description

**UCPP_CameraControls**: 

This class handles camera movement and positioning in the scene. It uses a spherical coordinate system to calculate the camera’s position and orientation relative to a target point. This class is purely computational and does not represent a physical object in the world. It can only be accessed through the `UCPP_User` class.


### Key Features:
- **Spherical Coordinate System**:  
  The camera’s position is calculated using polar (`Theta`), azimuthal (`Phi`), and radial (`R`) coordinates.  
  - **Theta (Polar)**: Controls vertical (up/down) movement.  
  - **Phi (Azimuth)**: Controls horizontal (left/right) movement.  
  - **Radius (R)**: Controls the distance from the target point.  
  The spherical coordinates are converted to Unreal Engine's Cartesian coordinate system for proper positioning.

---

## Definitions

### `ANATOM_PI` 

**Description**

Unreal Engine`s definition of PI is depricated so we fuck em. This makes calculating stuff much easier

----

## Public Methods

### `void Init(FVector LookAtPos, float maxRadius = 40.0f, float minRadius = 1.0f)`

**Description**:  
Initializes the orbit camera controls, defining the target point to orbit around and setting the radius limits.  

**Parameters**:  
- `LookAtPos`: The position in the world around which the camera should orbit.  
- `maxRadius`: The maximum distance the camera can be from the target. Default is `40.0f`.  
- `minRadius`: The minimum distance the camera can be from the target. Default is `1.0f`.  

---

### `void RotatePolar(float by)`

**Description**:  
Rotates the camera around the target vertically (up/down) by adjusting the polar angle (`Theta`).  

**Parameters**:  
- `by`: The rotation amount in radians.  

---

### `void RotateAzimuth(float by)`

**Description**:  
Rotates the camera around the target horizontally (left/right) by adjusting the azimuthal angle (`Phi`).  

**Parameters**:  
- `by`: The rotation amount in radians.  

---

### `FVector GetLookAtPos()`

**Description**:  
Retrieves the position of the target point the camera is orbiting.  

**Returns**:  
A `FVector` representing the target position in world coordinates.  

---

### `void MoveHorizontal(float by)`

**Description**:  
Moves the camera horizontally along the current orientation plane.  

**Parameters**:  
- `by`: The amount to move horizontally.  

---

### `void MoveVertical(float by)`

**Description**:  
Moves the camera vertically along the current orientation plane.  

**Parameters**:  
- `by`: The amount to move vertically.  

---

### `void Zoom(float by)`

**Description**:  
Adjusts the camera's distance from the target point by modifying the radius.  

**Parameters**:  
- `by`: The amount to adjust the radius. Positive values zoom out, and negative values zoom in.  

---

### `void SetLookAtTarget(FVector LookAtTarget)`

**Description**:  
Changes the target point the camera looks at.  

**Parameters**:  
- `LookAtTarget`: A new position in world coordinates for the camera to focus on.  

---

### `FVector3d &GetPosition()`

**Description**:  
Retrieves the current position of the camera in world space.  

**Returns**:  
A reference to the `FVector3d` representing the camera’s world position.  

---

### `FRotator &GetRotation()`

**Description**:  
Retrieves the current orientation of the camera.  

**Returns**:  
A reference to the `FRotator` representing the camera’s rotation.  

---

## Private Methods

### `FVector3d RecalculatePosition()`

**Description**:  
Recalculates the camera’s position in Cartesian coordinates based on the spherical coordinates:  

x = R * Cos(Phi) * Cos(Theta);
y = R * Cos(Phi) * Sin(Theta);
z = R * Sin(Phi); 

**Returns**:  
A `FVector3d` representing the updated camera position in world space.  

---

### `void RecalculateRotation()`

**Description**:  
Adjusts the camera’s orientation to ensure it follows the target location when moving or rotating.  

---

## Private Properties

### `FRotator Orientation`

**Description**:  
Represents the camera’s current orientation (rotation) in world space.  

---

### `FVector3d Location`

**Description**:  
Represents the camera’s current location in world space.  

---

### `float Radius`

**Description**:  
The current distance from the target point to the camera.  

---

### `float MaxRadius`

**Description**:  
The maximum allowed radius for the camera.  

---

### `float MinRadius`

**Description**:  
The minimum allowed radius for the camera.  

---

### `FVector LookAtPosition`

**Description**:  
The position in world space that the camera is currently focusing on.  

---

### `float AzimuthAngle`

**Description**:  
The azimuthal angle (`Phi`) representing horizontal rotation.  

---

### `float PolarAngle`

**Description**:  
The polar angle (`Theta`) representing vertical rotation.  

---

### `const float FullCircle`

**Description**:  
Represents a full circle in radians (\(2\pi\)). Defined as `ANATOMY_PI * 2.F`.  

---

## Notes

- The spherical coordinate system provides precise control over camera movement, allowing dynamic manipulation of angles and distance.  
- Ensure angles passed to methods like `RotatePolar` or `RotateAzimuth` are in radians for accurate results.  
- This class is designed to be accessed indirectly and is not meant to be placed directly in the world.  

## Further reading and resources 

[Great article about the orbit camera](https://www.mbsoftworks.sk/tutorials/opengl4/026-camera-pt3-orbit-camera/)