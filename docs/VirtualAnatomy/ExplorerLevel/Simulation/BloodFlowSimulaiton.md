# Blood flow simulation 

# Blood Flow Simulation Overview

This section describes how the blood flow simulation was developed.

## Mechanism of the Blood Flow Simulation

The core mechanism behind the blood flow simulation is that an actor can move smoothly along a predefined path, known as a spline in Unreal Engine. These splines are carefully hand-drawn to trace the most significant veins and arteries within the human body.

After creating these splines, we built an actor containing skeletal meshes of arteries and veins. Skeletal meshes allow for animation, which is essential for creating dynamic simulations. To organize the splines, we randomly selected the artery skeletal mesh to serve as the parent for all spline components, resulting in the following structure:

```plaintext
CirculatorySystem
    ├─ Veins skeletal mesh 
    ├─ Arteries skeletal mesh
    │   ├─ Spline
    │   ├─ Spline
    │   ├─ Spline 
    │   └─ Spline
```

This actor inherits from the CPP_BloodFlowSimulation class, allowing it to interact with the simulation manager and respond to simulation events.

To represent individual blood cells, we created a CPP_Cell class. This class includes a static mesh representing the blood cell, giving the class its name.

We then created a Blueprint that inherits from the CPP_Cell class. For maintainability, the implementation of the blood cell’s movement along the splines is handled within this Blueprint. For additional guidance on this implementation, you can refer to [this video](https://www.youtube.com/watch?v=-V6D5WtemMI). 

### Spawning cells
The Blueprint version of the cell is spawned into the world instead. If we were to spawn the CPP_Cell instead of the BP_Cell than logic behind moving alpng splines wpuld have to be implemented purely in CPP which would be rather painfull. To specify the blueprint version of the Blood Cell we have created a new member field in CPP_Cell that holds the type of the blueprint to spawn. This blueprint is assigned in the editor since the property is set to be EditAnywhere 

### Passing splines
The cell is unaware of what spline to follow to specifi the spline inside the CPP_Bloodflow we are itterating over all of the children of the aretoes skeleal mesh since thi is where splines are located then we retriece the pointer to this spline and we 

>**POSIBLE FUTURE IMPROVEMENT:** Currently, the splines are created manually using Unreal Engine’s spline creation tool. We trace the paths of arteries and veins, looping them through the heart region to simulate blood flow. While this setup creates a convincing illusion, an improvement would be to dynamically generate splines directly from the arteries and veins meshes. However, implementing this is challenging because the meshes are constructed from arrays of vertex positions with no inherent hierarchy. This lack of structure makes it difficult to automatically generate splines that follow the natural flow paths within these complex anatomical shapes. We have explored the possibility of voxelizing the meshes into a sparse voxel octree (SVO) structure. This approach could provide a more structured representation of the arteries and veins, potentially simplifying the process of spline generation by allowing the traversal of a hierarchical, voxel-based approximation of the mesh.  


