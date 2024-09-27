# Anatomy Helpers

This header file provides inline definitions of functions that help simplify the development process.

These functions are declared and defined under the `AnatomyUtils` namespace.

---

### `inline static AActor* GetActorByName(FString name, UWorld* world)`

**Parameters:**

- `FString name` - The name of the actor to search for.
- `UWorld* world` - The world in which to look for the actor.

**Returns:**

- `AActor*` - A pointer to the actor found. If the actor is not found, this function returns `nullptr` and a log message will inform you about the failure.

This helper function will serach the provide world for the actor with the provided name 

---


### `inline static AActor* GetRandomActor(UWorld* world)`


**Parameters:**

- `UWorld* world` - The world in which to look for the actor.

**Returns:**

- `AActor*` - A pointer to the random actor from the world, if world contains no actors this function will 
