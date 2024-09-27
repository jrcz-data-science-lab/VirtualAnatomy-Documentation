# Ray Caster

> **NOTE:** This class cannot be seen inside the Unreal Editor as it does not inherit any of the classes provided by Unreal Engine. Its main purpose is to abstract away the ray casting logic.


As mentioned, we have chosen ray casting as the primary method for selecting which objects the user clicks on.

The way it works is that when the user clicks on the screen, we receive coordinates about where the mouse was clicked (in screen space). We then transform this mouse location to world space and calculate the direction from our character to the point where the mouse clicked. This direction is normalized, meaning it has a length of 1.

Next, we specify a ray length parameter to determine where the ray should end.

After that, we shoot the ray from the position of our character towards the calculated end of the ray.

All of these calculations are performed once the user presses the left mouse button.

As you might imagine, all of this is happening in the `Click` event inside the `CPP_User` class.

The main responsibility of the `RayCaster` class is to trace the actual rays through the scene.

----

# `RayCaster.h`

## Public mehtods 

----

### `RayCaster(UWorld* world = nullptr)`

**Parameters:**

- `UWorld* world ` - world to use for ray casting  

The constructor takes a world as a parameter, which can be `nullptr` by default since the "world" does not exist during construction time.

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

### `bool TraceLine(FVector start, FVector rayDirection, float rayLength = 1000)`

**Parameters:**

- `FVector start` - The starting point of the ray in world space.
- `FVector rayDirection` - The direction in which the ray will travel.
- `float rayLength` - The length of the ray, default value is 1000.

**Returns:**

- `True` if a hit was detected.
- `False` otherwise.

This function uses the `LineTraceSingleByChannel` method from the `UWorld` class to trace the provided ray through the world. By default, it uses the **Visible** channel for intersection testing, meaning only objects that are visible within the scene will be tested for intersection.

If a hit is detected, the actor that caused the intersection will be stored in the `m_currentHitActor`.

Lastly, all actors that were added via `AddIngoredActor` method will be ignored

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



