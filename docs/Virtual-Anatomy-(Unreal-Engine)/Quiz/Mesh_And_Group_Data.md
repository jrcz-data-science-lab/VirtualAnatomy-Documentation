# FMeshCatalogEntryUE & FOrganGroupUE

* **File**: `VirtualAnatomy/Public/Quiz/MeshAndGroupData.h`
* **Purpose**: These structs are used by `UMeshKnowledgeBaseDataAsset` to store metadata about 3D anatomical meshes and their groupings, facilitating local lookups for "select organ" quiz questions.

## FMeshCatalogEntryUE

**Purpose**: Represents a single entry in the mesh catalog, mapping a mesh's name to its database ID and associated groups.

### Public Properties

### `FString DatabaseID`

**Type:** `FString`

The `_id` from MongoDB for this mesh entry.

---

### `FString MeshName`

**Type:** `FString`

The unique meshName identifier (e.g., "bones_Fourth_Rib_L").

---

### `FString DisplayName`

**Type:** `FString`

The name displayed in the UI (e.g., "Fourth Rib (L)"). This name conversion is handled by the API.

---

### `TArray<FString> OrganGroupDatabaseIDs`

**Type:** `TArray<FString>`

An array of MongoDB `_id` strings for groups this mesh belongs to.

---

### `int32 DefaultStudyYear`

**Type:** `int32`

Optional default study year for this mesh.

## FOrganGroupUE

**Purpose**: Represents a single entry for an organ group.

### Public Properties

### `FString DatabaseID`

**Type:** `FString`

The `_id` from MongoDB for the organ group.

---

### `FString GroupName`

**Type:** `FString`

The unique name of the organ group (e.g., "Leg").

---

### `int32 DefaultStudyYear`

**Type:** `int32`

Optional default study year for this group.
