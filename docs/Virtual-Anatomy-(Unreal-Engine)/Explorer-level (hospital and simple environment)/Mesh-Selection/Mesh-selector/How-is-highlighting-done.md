# Mesh Selector and highlighting

When the user clicks on a mesh, it gets highlighted. The Mesh Selector is responsible for determining which actor should be highlighted and which should not.

## How is Highlighting Done?

Highlighting different elements is achieved using a post-processing volume that spans the entire scene.

This post-processing volume uses the `HighLightMat` material, which can be found in `Content\Materials/`. To understand the process better, refer to [this video](https://www.youtube.com/watch?v=rGqlReFObYQ).

Every actor that has the `RenderCustomDepth` field set to `True` will be highlighted. By default, this field is set to `False`. We use this to our advantage by setting the `RenderCustomDepth` field to `True` whenever the user clicks on a mesh. This causes the clicked actor to be highlighted.

To improve performance, we have merged larger parts of the model (like bones, arteries and parts of hearth) in Blender, allowing us to render them all at once. You can find these in the `Body-Merged-Full` folder in the context menu. This helped us load all necessary textures without the hassle of exporting FBX files properly. As a result, Blender scripts used by previous developers are no longer needed.

To allow users to click on different parts of the model, we exported each body mesh as it is in the Blender file, but with fewer triangles using Blender's Decimate modifier.

Then, we hid these meshes, so they're loaded into Unreal Engine's Bounding Volume Hierarchy but not drawn. These hidden meshes (static mesh actors) are named `Body-Full-Picker`. This lets us detect clicks on different parts of the model without rendering them, which gave us a huge performance boost.

By default, the actors in Picker-Mesh are hidden and only become visible if the user selects them (clicks on them).

While this approach may cause more work for developers, it significantly improves the user experience.

To better organize the scene and the entire development environment, each major component is encapsulated within a blueprint class. For instance, bones are represented in the `BP_Bones` blueprint. This blueprint inherits from the `Actor` class, making it easy to retrieve later. Within this actor, you will find a Skeletal Mesh, which represents the combined structure of the human body. The Skeletal Mesh serves as the parent for all the `StaticMeshes`, which represent individual parts of the body for picking (e.g., bones, joints, etc.). Refer to the figure below for a visual understanding of this structure.


<figure markdown="span">
  ![bones and picker mesh](https://jrcz-data-science-lab.github.io/VirtualAnatomy-Documentation/images/picker-mesh-and-merged-mesh.png) <figcaption>Bones and its picker meshes</figcaption>
</figure>

With this approach you are able to retrieve the picker mesh in code or inside the blueprints by querying the all children of the skeletal mesh component.

!!! Warning

    Each merged part **MUST BE A SKELETAL MESH**, and each picker model **MUST BE A STATIC MESH AND CHILD OF THE SKELETAL MESH (MERGED BODY PART)**. This design choice was made to work within the constraints of the Unreal Engine’s class structure. Specifically, it is not possible to directly retrieve a `Static Mesh Component` (as opposed to static mesh actors) from a parent element.
    
    As a result, you cannot do something like this in code:

    ```c++
    
    //pseudo code 
    //===============
    
    AActor* actor = GetAllActorsWithTag("bones"); // get BP_Bones actor
    
    TMeshComponent mergedMesh = actor->GetAllComponets<UMeshComponent>()  // get top level components of type MeshComponent
    ```
    
    You might assume that the action will retrieve all top-level components that are of type or subclass of `UMeshComponent`, but this is not the case. Instead, it retrieves **all** components from the `BP_Bones` actor, including both Skeletal Mesh Component (for rendering) and Static Mesh components for the picker.
    
    If the merged model is not a Skeletal Mesh, this could make it difficult to isolate and retrieve either the picker mesh or the merged mesh directly. For example, you would need to loop through all the components and check manually for tags like `isPicker` or `isMerged`.
    
    This limitation is why the merged mesh is set as a Skeletal Mesh and the picker mesh as a Static Mesh. By using this approach, it becomes straightforward to identify whether the retrieved component is part of the picker mesh or the merged mesh.
    
    This approach is primarily used for user interaction and for organizing the tree view. Since the tree view only displays the merged meshes, we needed a way to organize them effectively. This is the solution we have chosen. However, if you find a better approach, feel free to implement it, but please make sure to update this documentation page accordingly.
