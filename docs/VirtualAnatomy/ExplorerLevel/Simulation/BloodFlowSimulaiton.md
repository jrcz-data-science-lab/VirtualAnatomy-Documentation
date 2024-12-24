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

This actor inherits from the `CPP_BloodFlowSimulation` class, allowing it to interact with the simulation manager and respond to simulation events.


> Following sections are depricated we have decided to use the Niagara particles to visualise the blood, I am keeping it here in case you need to know about this plese, consult new page that explains how niagara is utilised  visit [this page](docs/VirtualAnatomy/ExplorerLevel/BloodSimulation/HowBloodSimulationWorks.md)

To represent individual blood cells, we created a `CPP_Cell` class. This class includes a static mesh to represent the blood cell, which is why we named it `CPP_Cell`.

Next, we developed a Blueprint that inherits from the `CPP_Cell` class. For easier maintenance, we implemented the blood cell’s movement along splines within this Blueprint. For more information on this implementation, refer to [this video](https://www.youtube.com/watch?v=-V6D5WtemMI).

### Spawning Cells
In the simulation, the Blueprint version of the cell (`BP_Cell`) is spawned into the world. If we spawned `CPP_Cell` instead of `BP_Cell`, we would need to implement spline movement logic entirely in C++, which would be difficult without the help of Unreal’s timeline feature. 

To specify the Blueprint version of the blood cell, we added a new member field in `CPP_Cell` that holds the type of Blueprint to spawn. This Blueprint is assigned in the editor, as the property is set to `EditAnywhere`.

In addition the cells are spawn once during event begin play. If we were to spawn them every time when simulation starts and despawn them when it stops, it would be much more unefficient. 

### Passing Splines
The cell itself is unaware of which spline to follow. To specify the spline in `CPP_BloodFlowSimulation`, we iterate over all the children of the arterial skeletal mesh, as this is where the splines are located. We then retrieve a pointer to the `spline componet` and pass it the cell via the `	void SetSpline(TObjectPtr<USplineComponent> spline);` function 

> End of depricated section


 **Possible Future Improvement:** Currently, splines are created manually using Unreal Engine’s spline creation tool. We trace the paths of arteries and veins, looping them through the heart region to simulate blood flow. While this setup creates a convincing effect, an improvement would be to dynamically generate splines directly from the artery and vein meshes. However, implementing this is challenging, as these meshes are constructed from arrays of vertex positions with no inherent hierarchy. This lack of structure makes it difficult to automatically generate splines that follow the natural flow within these complex anatomical shapes. 
 We have explored voxelizing the meshes into a sparse voxel octree (SVO) structure. This approach could offer a more structured representation of the arteries and veins, potentially simplifying the process of spline generation by enabling traversal of a hierarchical, voxel-based approximation of the mesh.


