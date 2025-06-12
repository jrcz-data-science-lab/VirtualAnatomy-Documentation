# UMeshKnowledgeBaseDataAsset

* **File**: `VirtualAnatomy/Public/Quiz/MeshAndGroupData.h` & `VirtualAnatomy/Private/Quiz/MeshAndGroupData.cpp`
* **Purpose**: A Data Asset that acts as a local database for anatomical meshes and their associated groups. This is crucial for validating answers in "select organ" questions without needing a backend call for each click.

## Public Properties

### `TMap<FString, FMeshCatalogEntryUE> MeshCatalogByMeshName`

**Type:** `TMap<FString, FMeshCatalogEntryUE>`

Stores mesh catalog entries mapped by their unique `MeshName`.

---

### `TMap<FString, FOrganGroupUE> OrganGroupsByDatabaseID`

**Type:** `TMap<FString, FOrganGroupUE>`

Stores organ groups mapped by their `DatabaseID` (MongoDB `_id`).

## Public Methods

The below methods are helper functions that were used to populate the `DA_MeshKnowledgeBase` Data Asset. Will be useful if you want to add more meshes to the model & database, otherwise not used anywhere

### `const FMeshCatalogEntryUE* GetMeshEntryByDatabaseID(const FString& InDatabaseID) const`

**Parameters:**

-   `const FString& InDatabaseID` - The database ID of the mesh to retrieve.

Helper function to get a mesh entry by its database ID.

---

### `void ClearAllKnowledgeBaseData()`

Clears all stored mesh catalog and organ group data within the asset.

---

### `void AddOrganGroupEntry(const FString& DatabaseID, const FString& GroupName, int32 DefaultStudyYear)`

**Parameters:**

-   `const FString& DatabaseID` - The MongoDB `_id` for the organ group.
-   `const FString& GroupName` - The unique name of the organ group.
-   `int32 DefaultStudyYear` - The default study year for this group.

Adds a new organ group entry to the knowledge base.

---

### `void AddMeshCatalogEntry(const FString& MeshNameKey, const FString& EntryDatabaseID, const FString& EntryMeshName, const FString& EntryDisplayName, const TArray<FString>& EntryOrganGroupDatabaseIDs, int32 EntryDefaultStudyYear)`

**Parameters:**

-   `const FString& MeshNameKey` - The key for the `MeshCatalogByMeshName` TMap.
-   `const FString& EntryDatabaseID` - The `_id` from MongoDB for this mesh entry.
-   `const FString& EntryMeshName` - The meshName field for the struct.
-   `const FString& EntryDisplayName` - The name displayed in the UI.
-   `const TArray<FString>& EntryOrganGroupDatabaseIDs` - Array of MongoDB `_id` strings of groups this mesh belongs to.
-   `int32 EntryDefaultStudyYear` - The default study year for this mesh entry.

Adds a new mesh catalog entry to the knowledge base.
