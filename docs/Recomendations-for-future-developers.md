---
weight: 2
---

# Recommendations for future developers

## Recommendations from [@bartan02]((https://github.com/Bartan02))

### Version Control
When working with Unreal Engine assets (especially with this one: ExplorerMap.uasset), developers must be extremely cautious with branch management. Concurrent modifications to the same asset can lead to difficult merge conflicts.
Additionally, To maintain project integrity and reduce conflicts, changes should be committed, pulled, and merged frequently—especially after major updates or before switching tasks.

### Simulation Manager
Significant improvements to the simulation system (managed by SimulationManager class), are recommended:

- Investigate automated methods for spline generation to better simulate blood particle flow.
- Use the predefined poses available in the project to support a fully functional and position-aware anatomy simulation system.
Do not forget about dynamic organ positioning and simulating corresponding blood flow variations based on different body poses. (See more: [Body positions](Virtual-Anatomy-(Unreal-Engine)/Explorer-level%20(hospital%20and%20simple%20environment)/Animations/Body-positions.md))
- Last, but not least, minimize the frequency of crashes that occur when interacting with this system.

### Slicer
Implementing logic to exclude raycasting on body parts hidden by the slicer could be considered. While not critical, this enhancement could improve performance and offer a smoother, more intuitive user experience, especially during interactions with the visible anatomy layers.

### New features
There are still some pending new features, such as the inclusion of blood pressure functionality and additional human parts or organs suggested by the nursing faculty. These enhancements would be valuable additions to the application. For some more details, please go to '(User) requirements list' Issue on GitHub repository.

??? "Recommendations left on version 1.0.0 of this documentation"

    This seciton covers some recomendations we have for you. If you find this taken out of context, it is. I have written this to my porfolio and i am putting it here for you so that you have some stepping stones while starting this project and to know, what should you focuse on. I hope i have covered most of the aspects that needs to be done. Keep in mind that this is only my (Simon's) recomendations. Daniel will hopefuly have his own paragraph later on. 
    
     
    The final recommendation is to continue with the project. Since the human body is a highly complex organism, more than six months are needed to fully translate it into a digital version. While the anatomical model was provided, it was not initially suited for interactive 3D applications. Consequently, significant reduction in the model's triangle count was implemented. Although these optimizations made the model more suitable for interactive rendering, they reduced the model's quality. It is recommended to employ more advanced triangle reduction techniques to achieve better performance while maintaining or improving model quality.
    
    The Niagara system, which simulates blood, was implemented later in the process. Therefore, additional optimizations and general improvements should be prioritized. For instance, collision detection currently uses ray tracing, which has a significant performance cost. A more approximate solution, such as analytical planes, could be implemented for particle/plane collisions. Since particles are represented as sprites, this issue can effectively be reduced to plane/plane collision handling.
    
    Draw call reduction—achieved by merging parts of the model—enabled the application to render a single frame in 8ms, which is excellent and does not require further optimization. However, this decision made it challenging to isolate specific parts of the body for display. For instance, it is currently impossible to display only the upper body. A recommended solution is to use a technique called stencil masking, which modifies stencil buffer values to force the renderer to exclude certain model parts from being displayed.
    
    The underlying architecture has been designed to be future-proof. Various design patterns were utilized, and a small UI framework was developed. It is recommended that future developers expand this UI framework to ensure seamless communication with underlying systems like simulation and animations.
    
    Given the project's scale, new developers should initially focus on resolving smaller bugs to familiarize themselves with the codebase. For example, one such bug is the camera deadlock that occurs when the user gets too close to the body and performs specific movements, preventing further navigation.
    
    Since it was agreed with key stakeholders that the primary focus should be on the "Explorer Level" of the Unreal Engine application, future developers are advised to concentrate on the quiz feature and its integration with the website. The quiz feature could benefit from the existing UI framework to achieve a modern look and streamline the automation of quiz question creation. The login and registration system also require improvements, and the quiz creation web application should be heavily inspired by the "Wooclap" platform, as per client feedback.
    
    Lastly, future developers should explore automated methods for creating splines used for blood particle movement. One promising approach is voxelization, which involves constructing a sparse voxel octree. The centers of individual voxels can then serve as control points for the splines.
    
    For the employer, it is recommended to hire individuals skilled in animation. The current solution uses superficial animated body parts that do not accurately represent the internal processes of the human body. Thanks to the application’s modular structure, it is straightforward to replace animations and animation assets without disrupting the existing solution.
    
    Last area of focus is ray-casting, which involves clicking on the model and identifying the selected parts. While this functionality works reasonably well, it is not the most accurate. One potential improvement is to increase the scale of the model and the picker model, creating a larger surface area for more precise selection of small meshes. However, this change would require adjustments to the camera’s field of view (FOV) and other tweaks to ensure the model does not appear oversized. These tweaks may require further exploration to implement effectively.
