# Ray Caster

> **NOTE:** This class cannot be seen inside the Unreal Editor as it does not inherit any of the classes provided by Unreal Engine. Its main purpose is to abstract away the ray casting logic.


As mentioned, we have chosen ray casting as the primary method for selecting which objects the user clicks on.

The way it works is that when the user clicks on the screen, we receive coordinates about where the mouse was clicked (in screen space). We then transform this mouse location to world space and calculate the direction from our character to the point where the mouse clicked. This direction is normalized, meaning it has a length of 1.
After that, we specify a ray length parameter to determine where the ray should end and then cast the ray from the character’s position towards the calculated endpoint.

Previously, the system used basic ray-object collision detection techniques, but we have now implemented the Möller-Trumbore algorithm for more precise intersection testing with triangles in the scene. This algorithm allows efficient ray-triangle intersection detection, making it particularly useful for accurate selection of 3D models and surfaces.

All of these calculations are performed once the user presses the left mouse button.

As you might imagine, all of this is happening in the `Click` event inside the `CPP_User` class.

The main responsibility of the `RayCaster` class is to trace the actual rays through the scene.

----

# `RayCaster.h`

## Public mehtods 

----

### `RayCaster()`

Constructor initializing the m_world and m_currentHitComponent to nullptr.

`m_world` is set initially to `nullptr` since the "world" does not exist during construction time.

However, you can construct this class in other places besides constructors.

This decision gives you the flexibility to do so.

The world is used to receive data from the scene so that we can test for the intersection with the ray.

----

### `void AddIgnoredActor(AActor* actorToIgnore);`

**Parameters:**

- `AActor* actorToIgnore` - pointer to the actor that you want to ignore 


Ignores the actor provided as the parameter. The parameter accepts a pointer to the actor.

----

### `void AddIgnoredActor(FString nameOfActorToIgnore);`

**Parameters:**

- `FString nameOfActorToIgnore` - name of the actor that you want to get ignored 

Ignores the actor provided as the parameter. The parameter accepts the name of the actor. The name of this actor is looked up in the world and ignored. If the actor is not found, a message in the log will inform you about this.

----

### `void SetWorld(UWorld *world)`

**Parameters:**

- `UWorld *world` - world to be set

Sets the current world that is going to be used during intersection testing. This will overwrite any other stored world inside the `m_world` member.

This setter was chosen to preserve OOP principles and allow for the construction of this class during the construction time, during which the world is not available.

Therefore, before each intersection test, you must ensure that the world is set correctly.

----

### `bool TraceLine(FVector start, FVector rayDirection, float rayLength, FVector& outHitPoint, FVector& outHitNormal, int32& outTriangleIndex, float& outU, float& outV)`

Performs a precise ray trace, detecting triangle-level intersections on static meshes.

**Parameters:**

- `start`- the starting point of the ray
- `rayDirection`- the direction of the ray
- `rayLength`- the length of the ray
- `outHitPoint`-  stores the location where the ray hits
- `outHitNormal`- stores the normal of the hit surface
- `outTriangleIndex`- stores the index of the hit triangle (-1 if no triangle was hit)
- `outU, outV`- Barycentric coordinates of the hit point

**Returns:**

- `True` if an intersection is found.
- `False` otherwise.

This function uses the `LineTraceSingleByChannel` method from the `UWorld` class to trace the provided ray through the world. By default, it uses the **Visible** channel for intersection testing, meaning only objects that are visible within the scene will be tested for intersection.

If a hit is detected, the actor that caused the intersection will be stored in the `m_currentHitActor`.

Lastly, all actors that were added via `AddIngoredActor` method will be ignored

---

### `UMeshComponent* GetCurrentHitComponent() const`

Returns the mesh component that was hit by the last raycast.


## Private methods

---

### `bool RayTriangleIntersect(const FVector& rayOrigin, const FVector& rayDirection, const FVector& v0, const FVector& v1, const FVector& v2, float& outDistance, float& outU, float& outV, FVector& outNormal)`

Uses the Möller-Trumbore algorithm to determine if a ray intersects a specific triangle in a mesh.

**Parameters:**
- `rayOrigin (FVector)`-  the starting point of the ray.
- `rayDirection (FVector)`- the normalized direction vector of the ray.
- `v0 (FVector)`- the first vertex of the triangle.
- `v1 (FVector)`- the second vertex of the triangle.
- `v2 (FVector)`- the third vertex of the triangle.
- `outDistance (float)`- the distance from the ray origin to the intersection point (if any).
- `outU (float)`- the barycentric coordinate U of the intersection point.
- `outV (float)`- the barycentric coordinate V of the intersection point.
- `outNormal (FVector)`- the normal of the intersected triangle.

**Returns:**
- `true` if the ray intersects the triangle.
- `false` otherwise.
---

### `bool GetTriangleVertices(UStaticMeshComponent* staticMeshComp, int32 triangleIndex, FVector& outV0, FVector& outV1, FVector& outV2)`

Retrieves the world-space vertices of a given triangle from a static mesh.

**Parameters:**
- `staticMeshComp (UStaticMeshComponent)`- the static mesh component from which to retrieve triangle data.
- `triangleIndex (int32)`- the index of the triangle within the mesh.
- `outV0 (FVector)`- the first vertex of the triangle in world space.
- `outV1 (FVector)`- the second vertex of the triangle in world space.
- `outV2 (FVector)`- the third vertex of the triangle in world space.

**Returns:**
- `true` if the triangle vertices were successfully retrieved.
- `false` otherwise.

---

### `void DrawOBB(UMeshComponent* meshComponent, float duration = 2.0f, float thickness = 2.0f)`

Draws the Oriented Bounding Box (OBB) for debugging purposes.

**Parameters:**
- `meshComponent (UMeshComponent)`- the mesh component to visualize.
- `duration (float, optional)`- the duration (in seconds) for which the debug box remains visible. *(Default: 2.0s)*
- `thickness (float, optional)`- the line thickness of the debug box. *(Default: 2.0)*

## Private members

---

### `TArray<AActor*> m_ignoredActors`

Array of the actors that are curretntly removed 

>**TODO**: add method to delete actors from this array 

---

### `FVector m_start`

Start of the ray in the world space 

---

### `FVector m_End`

End of the ray in the world space 

---

### `UWorld* m_world`

World in which the ray casting and intersection testing will take place 

---

### `AActor* m_currentHitActor`

Actor that was hit by the constructed ray. This is `nullptr` if no actor was hit.

---

### `int32 m_lastHitTriangleIndex`

Index of the last hit triangle in the static mesh.

---

### `FVector m_lastHitPoint`

World-space position of the last successful hit.

---

### `FVector m_lastHitNormal`

Normal of the last successfully hit triangle.

