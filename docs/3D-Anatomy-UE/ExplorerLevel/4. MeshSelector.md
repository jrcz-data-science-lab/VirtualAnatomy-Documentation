# Mesh Selector

When the user clicks on a mesh, it gets highlighted. This class is responsible for determining which actor should be highlighted and which should not.

## How is Highlighting Done?

Highlighting different elements is achieved using a post-processing volume that spans the entire scene.

This post-processing volume uses the `HighLightMat` material, which can be found in `Content\Materials/`. To understand the process better, refer to [this video](https://www.youtube.com/watch?v=rGqlReFObYQ).

Every actor that has the `RenderCustomDepth` field set to `True` will be highlighted. By default, this field is set to `False`. We use this to our advantage by setting the `RenderCustomDepth` field to `True` whenever the user clicks on a mesh. This causes the clicked actor to be highlighted.

# `MeshSelector.h`

This class is responsible for performing the actions specified above.

## Public methods 

---

### `MeshSelector()`

The constructor of this class is empty as no parameters need to be passed during instantiation.

---

### `void HighlightActor(AActor* actor)`

**Parameters:**

- `AActor* actor` - The actor that was clicked and should be highlighted.

This function highlights the actor passed as the parameter and de-highlights the actor that was previously highlighted.

Additionally, it stores the previously highlighted actor inside `m_previouslySelectedActor` for future reference.

---

### `void DeselectAllActors()`

Deselets all of the actors that were hightlighted 

---

## Private Members

---

### `AActor* m_currentlySelectedActor`

The actor that is currently highlighted. By default, this is set to `nullptr` or is `nullptr` when nothing is selected.

---

### `AActor* m_previouslySelectedActor`

The actor that was previously highlighted. By default, this is set to `nullptr` or is `nullptr` when nothing is selected.

---

## Private Methods 

### `void SetActorHighlightVisible(AActor* actor, bool isHighlightVisible)`

**Parameters**:

- `AActor* actor` - The actor on which the highlight should be visible or hidden.

- `bool isHighlightVisible` - Determines whether the highlight should be visible or hidden for this actor. `true` if the highlight should be visible, `false` otherwise.

Helper function that is setting the value `RenderCustomDepth` for the actor passed as the parameter 

