# Animations

So far, all body parts that have been loaded are animated. All animations are stored in the `Body-Merged-Full/Animations` directory, which contains folders for each body part. These folders include various animation-related files. For example, in the `Body` folder, you will find the `Skeleton` for the bones mesh, the `Animation Blueprint`, and the `Level Sequence`.

We have only rigged and animated the merged meshes that are displayed in the viewport.

## How We Animated

1. **Importing Merged Meshes**:  
   We imported the merged meshes from Blender. Once imported, we right-clicked on the desired mesh and selected `Convert to Skeletal Mesh`.  
   *Note: If you don't see this option, enable the plugin by going to `Plugins -> Skeletal Mesh Editing Tools`.*

2. **Creating the Skeletal Mesh**:  
   This conversion generated the skeletal mesh, which can then be rigged by placing bones and adjusting the weights of the mesh's vertices.  
   For more information on rigging, please refer to [this video](https://www.youtube.com/watch?v=kKKYeMVvNvg).

3. **Creating the Animation**:  
   To animate the model, we created a sequencer by right-clicking inside the content browser.  
   It is important to load the skeleton (not the skeletal mesh) into the sequencer.

4. **Baking and Playing the Animation**:  
   Once we were satisfied with the animation, we baked it into an animation asset. 

5. **Creating Animation Blueprint**:
   To access animations in C++, we created a C++ class for each animation and then generated an animation blueprint from these classes. The animations are located inside the `Animations` folder.

> **Important:** Each C++ animation class must inherit from the `BaseAnimation` class, allowing us to manipulate the animations during the simulation.

## Naming Conventions Used

- `SK_` prefix for skeletons.
- `CamelCase` for baked animation sequences.
- `SEQ_` prefix for sequencers.
- `BPA_` prefix for animation blueprints.

