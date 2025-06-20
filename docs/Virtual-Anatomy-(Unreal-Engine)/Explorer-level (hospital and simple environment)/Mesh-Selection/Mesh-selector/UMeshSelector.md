# `UMeshSelector`

## Class Description

This class is responsible for highlighting mesh components that represent anatomical parts selected by the user. Additionally, it interacts with the ClickedOnInfo UI to display details about the selected anatomical part.

[Clicked on info UI compoonent](../UI/UIClasses/ClickedOnInfo.md)

## Public Methods

### `MeshSelector()`

**Description**:

The constructor of this class is empty, as no parameters need to be passed during instantiation.

### `void Init(UWorld* world)`

**Description**:

This method initializes the `MeshSelector` with the necessary world context, allowing it to interact with world-specific elements.

**Parameters:**

- `UWorld* world` - The world context that this selector will operate within.

### `void HighlightActor(AActor* actor)`

**Description**:

**Deprecated.** This function highlights the specified actor and removes the highlight from any previously highlighted actor. It also stores the previously highlighted actor in `m_previouslySelectedActor`.

**Parameters:**

- `AActor* actor` - The actor to highlight when clicked.

### `void HighlightComponent(UMeshComponent* mesh)`

**Description**:

Highlights the specified mesh component, adding a visual effect to indicate selection. It de-highlights any previously highlighted mesh component and updates the reference to `m_previouslySelectedMesh`.

**Parameters:**

- `UMeshComponent* mesh` - The mesh component to highlight.

### `void DeselectAllActors()`

**Description**:

**Deprecated.** Deselects all highlighted actors, removing any active visual highlights.

### `void DeselectAllComponents()`

**Description**:

Deselects all highlighted mesh components, removing any active visual highlights.

## Private Members

### `AActor* m_currentlySelectedActor`

**Description**:

A reference to the actor currently highlighted. Defaults to `nullptr` if no actor is highlighted.

### `AActor* m_previouslySelectedActor`

**Description**:

A reference to the previously highlighted actor. Defaults to `nullptr` if no actor was previously selected.

### `UMeshComponent* m_currentlySelectedMesh`

**Description**:

Stores a reference to the currently highlighted mesh component. Defaults to `nullptr` if no mesh is highlighted.

### `UMeshComponent* m_previouslySelectedMesh`

**Description**:

Stores a reference to the previously highlighted mesh component. Defaults to `nullptr` if no mesh was previously selected.

### `UCPP_ClickedOnInfo* m_displayedClickOnNameWidget`

**Description**:

Reference to the ClickedOnInfo widget that displays information about the currently selected anatomical part.

### `UWorld* m_world`

**Description**:

The world context for this selector, used to interact with in-world components.

## Private Methods

### `void SetActorHighlightVisible(AActor* actor, bool isHighlightVisible)`

**Description**:

**Deprecated.** This helper function adjusts the `RenderCustomDepth` value for the actor to control the highlight visibility.

**Parameters:**

- `AActor* actor` - The actor on which the highlight should be shown or hidden.
- `bool isHighlightVisible` - Sets whether the highlight is visible (`true`) or hidden (`false`).

### `void SetComponentHighlightVisible(UMeshComponent* mesh, bool isHighlightVisible)`

**Description**:

This helper function controls the highlight visibility for the specified mesh component, using `RenderCustomDepth` to achieve the effect.

**Parameters:**

- `UMeshComponent* mesh` - The mesh component for which the highlight should be shown or hidden.
- `bool isHighlightVisible` - Determines whether the highlight is visible (`true`) or hidden (`false`).
