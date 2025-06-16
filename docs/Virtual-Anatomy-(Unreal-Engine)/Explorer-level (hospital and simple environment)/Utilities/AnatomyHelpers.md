# AnatomyUtils 

A namespace containing utility functions for operations related to anatomy exploration in the context of a game or simulation. These functions are specifically designed to work reliably at the explorer level. While most functions are robust, some may return `nullptr` under specific conditions, which requires caution during use.

---

## Public Methods 

### `AActor* GetActorByName(FString& ActorName, UWorld* World)`

**Description**:  
Retrieves an actor from the world by its name.  

**Parameters**:  
- `ActorName`: The name of the actor to search for.  
- `World`: A pointer to the world where the search should occur.  

**Returns**:  
A pointer to the found actor. If the actor is not found, the function returns `nullptr`. Exercise caution when handling `nullptr` results.  

---

### `AActor* GetRandomActor(UWorld* World)`

**Description**:  
Selects and retrieves a random actor from the scene.  

**Parameters**:  
- `World`: A pointer to the world from which the actor will be retrieved.  

**Returns**:  
A pointer to a random actor. If the scene is empty, the function returns `nullptr`.  

---

### `TObjectPtr<UCPP_SimulationManager> GetSimulationManager(UWorld* World)`

**Description**:  
Retrieves an instance of the simulation manager from the GameMode associated with the provided world.  

**Parameters**:  
- `World`: The world in which the GameMode contains the simulation manager instance.  

**Returns**:  
A `TObjectPtr<UCPP_SimulationManager>` pointing to the simulation manager. If the simulation manager is not present in the GameMode, the function returns `nullptr`.  

---

### `TObjectPtr<UCPP_IsolateMesh> GetIsolateMeshInstance(UWorld* World)`

**Description**:  
Obtains an instance of the mesh isolator class from the GameMode.  

**Parameters**:  
- `World`: The world in which the GameMode contains the mesh isolator instance.  

**Returns**:  
A `TObjectPtr<UCPP_IsolateMesh>` pointing to the mesh isolator class. If the instance is not present in the GameMode, the function returns `nullptr`.  

---

### `TObjectPtr<UMeshSelector> GetMeshSelector(UWorld* World)`

**Description**:  
Retrieves the mesh selector instance from the user character in the specified world.  

**Parameters**:  
- `World`: The world containing the user character (instance of `UCPP_User`).  

**Returns**:  
A `TObjectPtr<UMeshSelector>` pointing to the mesh selector instance. If the world does not contain an instance of `UCPP_User`, the function returns `nullptr`.  

---

### `float DegTo# AnatomyUtils

A namespace containing utility functions for operations related to anatomy exploration in the context of a game or simulation. These functions are specifically designed to work reliably at the explorer level. While most functions are robust, some may return `nullptr` under specific conditions, which requires caution during use.

---

## Public Methods

### `AActor* GetActorByName(FString& ActorName, UWorld* World)`

**Description**:  
Retrieves an actor from the world by its name.

**Parameters**:
- `ActorName`: The name of the actor to search for.
- `World`: A pointer to the world where the search should occur.

**Returns**:  
A pointer to the found actor. If the actor is not found, the function returns `nullptr`. Exercise caution when handling `nullptr` results.

---

### `AActor* GetRandomActor(UWorld* World)`

**Description**:  
Selects and retrieves a random actor from the scene.

**Parameters**:
- `World`: A pointer to the world from which the actor will be retrieved.

**Returns**:  
A pointer to a random actor. If the scene is empty, the function returns `nullptr`.

---

### `TObjectPtr<UCPP_SimulationManager> GetSimulationManager(UWorld* World)`

**Description**:  
Retrieves an instance of the simulation manager from the GameMode associated with the provided world.

**Parameters**:
- `World`: The world in which the GameMode contains the simulation manager instance.

**Returns**:  
A `TObjectPtr<UCPP_SimulationManager>` pointing to the simulation manager. If the simulation manager is not present in the GameMode, the function returns `nullptr`.

---

### `TObjectPtr<UCPP_IsolateMesh> GetIsolateMeshInstance(UWorld* World)`

**Description**:  
Obtains an instance of the mesh isolator class from the GameMode.

**Parameters**:
- `World`: The world in which the GameMode contains the mesh isolator instance.

**Returns**:  
A `TObjectPtr<UCPP_IsolateMesh>` pointing to the mesh isolator class. If the instance is not present in the GameMode, the function returns `nullptr`.

---

### `TObjectPtr<UMeshSelector> GetMeshSelector(UWorld* World)`

**Description**:  
Retrieves the mesh selector instance from the user character in the specified world.

**Parameters**:
- `World`: The world containing the user character (instance of `UCPP_User`).

**Returns**:  
A `TObjectPtr<UMeshSelector>` pointing to the mesh selector instance. If the world does not contain an instance of `UCPP_User`, the function returns `nullptr`.

---

### `float ConvertBeatsPerMinuteToBeatsPerSecond(const float &BeatsPerMinute, float MaxPossibleBPS = 5)`;

**Description**:  
Calculates beats per minute to the beats per second

**Parameters**:
- `BeatsPerMinute`: beats per minute of the heart
- `MaxPossibleBPS`: maximum beats per seconds given in the Types/SlideBarParameter.h, we have to suply this number to properly scale how often "hearth releases new particles" default value is 5 since max possible BPM is 300 and 300 BPM / 60 sec = 5 BPS

**Returns**:  
How often should CPP_BloodParticleSystem tick, use this value to configure tick frequency.

---

### `float ConvertDegreesToRadians(float Angle)`

**Description**:  
Converts an angle from degrees to radians.

**Parameters**:
- `Angle`: The angle in degrees.

**Returns**:  
The angle converted to radians.

---


## Notes

- Ensure the validity of pointers returned by these functions before usage to avoid null pointer dereferencing.  
- These utilities are intended for operations within the explorer level, and their reliability assumes the context is set up correctly.