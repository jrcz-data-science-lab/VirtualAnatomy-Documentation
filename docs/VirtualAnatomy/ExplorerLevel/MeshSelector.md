# Mesh Selector

When the user clicks on a mesh, it gets highlighted. This class is responsible for determining which actor should be highlighted and which should not.

## How is Highlighting Done?

Highlighting different elements is achieved using a post-processing volume that spans the entire scene.

This post-processing volume uses the `HighLightMat` material, which can be found in `Content\Materials/`. To understand the process better, refer to [this video](https://www.youtube.com/watch?v=rGqlReFObYQ).

Every actor that has the `RenderCustomDepth` field set to `True` will be highlighted. By default, this field is set to `False`. We use this to our advantage by setting the `RenderCustomDepth` field to `True` whenever the user clicks on a mesh. This causes the clicked actor to be highlighted.

To improve performance, we've merged larger parts of the model (like bones, arteries and parts of hearth) in Blender, allowing us to render them all at once. You can find these in the `FullBody_Merged` folder in the context menu. This helped us load all necessary textures without the hassle of exporting FBX files properly. As a result, Blender scripts used by previous developers are no longer needed.

To allow users to click on different parts of the model, we exported each body mesh as it is in the Blender file, but with fewer triangles using Blender's Decimate modifier.

We then hid these meshes, so they're loaded into Unreal Engine's Bounding Volume Hierarchy but not drawn. These hidden meshes (static mesh actors) are named `Picker-Mesh`. This lets us detect clicks on different parts of the model without rendering them, which gave us a huge performance boost.

By default, the actors in Picker-Mesh are hidden and only become visible if the user selects them.

While this approach may cause more work for developers, it significantly improves the user experience.

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

