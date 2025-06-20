# `FRayCaster`

## Public mehtods

### `FRayCaster()`

**Description:**

Constructor initializing the `World` and `CurrentHitComponent` to nullptr.

`World` is set initially to `nullptr` since the "world" does not exist during construction time.

However, you can construct this class in other places besides constructors.

This decision gives you the flexibility to do so.

The world is used to receive data from the scene so that we can test for the intersection with the ray.

### `void AddIgnoredActor(AActor* ActorToIgnore);`

**Description:**

Ignores the actor provided as the parameter. The parameter accepts a pointer to the actor.

**Parameters:**

- `AActor* ActorToIgnore` - pointer to the actor that you want to ignore

### `void AddIgnoredActor(FString NameOfActorToIgnore);`

**Description:**

Ignores the actor provided as the parameter. The parameter accepts the name of the actor. The name of this actor is looked up in the world and ignored. If the actor is not found, a message in the log will inform you about this.

**Parameters:**

- `FString NameOfActorToIgnore` - name of the actor that you want to get ignored

### `void SetWorld(UWorld *WorldToSet)`

**Parameters:**

- `UWorld *WorldToSet` - world to be set

Sets the current world that is going to be used during intersection testing. This will overwrite any other stored world inside the `World` member.

This setter was chosen to preserve OOP principles and allow for the construction of this class during the construction time, during which the world is not available.

Therefore, before each intersection test, you must ensure that the world is set correctly.

### `bool TraceLine(FVector RayOrigin, FVector RayDirection, float RayLength, FVector& OutHitPoint, FVector& OutHitNormal, int32& OutTriangleIndex, float& OutU, float& OutV)`

**Description:**

This function performs a high-precision raycast against static mesh geometry in the world. It first uses Unreal Engine’s built-in line trace to detect if any object is hit along the ray path. If a hit occurs, it retrieves the mesh data of the impacted component and performs a detailed per-triangle intersection test (inside `RayTriangleIntersect` function) using the Möller–Trumbore algorithm to identify the exact triangle hit by the ray. 
This is done in parallel to improve performance. If a triangle intersection is found within the ray’s length, it returns the closest hit point and related data. If no precise triangle match is found, it falls back to the default hit information from the line trace.

**Parameters:**

- `RayOrigin`- the starting point of the ray
- `RayDirection`- the direction of the ray
- `RayLength`- the length of the ray
- `OutHitPoint`-  stores the location where the ray hits
- `OutHitNormal`- stores the normal of the hit surface
- `OutTriangleIndex`- stores the index of the hit triangle (-1 if no triangle was hit)
- `OutU, OutV`- Barycentric coordinates of the hit point

**Returns:**

- `True` if an intersection is found.
- `False` otherwise.

All actors that were added via `AddIngoredActor` method will be ignored.

### `UMeshComponent* GetCurrentHitComponent() const`

Returns the mesh component that was hit by the last ray cast.

## Private methods

### `bool RayTriangleIntersect(const FVector& RayOrigin, const FVector& RayDirection, const FVector& Vector0, const FVector& Vector1, const FVector& Vector2, float& OutDistance, float& OutU, float& OutV, FVector& OutNormal)`

**Description:**

Uses the Möller-Trumbore algorithm to determine if a ray intersects a specific triangle in a mesh.

**Parameters:**

- `RayOrigin (FVector)`-  the starting point of the ray.
- `RayDirection (FVector)`- the normalized direction vector of the ray.
- `Vector0 (FVector)`- the first vertex of the triangle.
- `Vector1 (FVector)`- the second vertex of the triangle.
- `Vector2 (FVector)`- the third vertex of the triangle.
- `OutDistance (float)`- the distance from the ray origin to the intersection point (if any).
- `OutU (float)`- the barycentric coordinate U of the intersection point.
- `OutV (float)`- the barycentric coordinate V of the intersection point.
- `OutNormal (FVector)`- the normal of the intersected triangle.

**Returns:**

- `true` if the ray intersects the triangle.
- `false` otherwise.

## Private members

### `TArray<AActor*> IgnoredActors`

**Description:**

Array of the actors that are currently removed

### `UWorld* World`

**Description:**

World in which the ray casting and intersection testing will take place

### `int32 LastHitTriangleIndex`

**Description:**

Index of the last hit triangle in the static mesh.

This is one of debug data for last hit. Those are not used in the current implementation, but can be useful for debugging purposes.


### `FVector LastHitPoint`

**Description:**

World-space position of the last successful hit.

This is one of debug data for last hit. Those are not used in the current implementation, but can be useful for debugging purposes.

### `FVector LastHitNormal`

**Description:**

Normal of the last successfully hit triangle.

This is one of debug data for last hit. Those are not used in the current implementation, but can be useful for debugging purposes.
