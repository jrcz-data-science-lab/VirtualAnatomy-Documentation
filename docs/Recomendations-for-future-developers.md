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

---

### Recommendations from @animepuika

#### 🧑‍🏫 Usability & UI Enhancements

- Develop a tutorial or onboarding level to guide first-time users through:
  - Toggling anatomical layers
  - Using the slicer
  - Interacting with simulation indicators
- Automatically activate the slicer menu when accessed to reduce confusion.
- Fully localize the user interface into Dutch, including:
  - Quiz buttons
  - Feedback messages
  - Interface text

#### ⚙️ System Stability & Technical Refactoring

- Resolve crash behavior in the `SimulationManager`, especially during:
  - Complex state transitions
  - Niagara FX spawning or cleanup
- Refactor the simulation core to:
  - Improve performance
  - Increase runtime stability
  - Simplify behavior switching logic
- Consolidate duplicated logic related to rendering and simulation parameters across diagnosis behaviors.

#### 🔬 Functional Expansion & Dynamic Simulation

- Enhance the blood circulation system to simulate:
  - Dynamic blood pressure and flow changes per diagnosis
- Expand the shock system to include:
  - Vitals integration (e.g. BPM and blood thickness)
  - Real-time updates to temperature indicators
- Enable mesh-based deformation of blood vessels:
  - Visually simulate shrinking/enlarging based on selected shock type
- Standardize Niagara FX triggering across all diagnoses for consistency and modularity.

> These improvements aim to evolve the Virtual Anatomy system into a scalable, production-ready tool for medical education.
 
---

### Recommendations from [@MGreizis](https://github.com/MGreizis)

  A significant technical challenge that was encountered during the development of the quiz module was how tightly coupled the existing application was. The initial, and architecturally cleanest, approach was to create a distinct _QuizGameMode_ and _QuizPlayerController_. This would have encapsulated all quiz logic, clearly separating it from the simulation functionalities of the main application. An attempt was made to do so, however this approach revealed deep-rooted dependencies that made this separation of concerns not feasible within the iteration’s timeframe. The core of the issue lies in the fact that the _ACPP_User_ and _ACPP_GameMode_ classes serve as “pillars” of the entire application, which in turn creates a web of dependencies. For a visual overview of the problem, see the following image:

![image](https://github.com/user-attachments/assets/134550a1-2d9a-4e7a-a02d-1066a6ec3a61)

 -  **The Game Mode**: The ACPP_GameMode is not just where the rules of the level are set; it also acts as a manager factory. It instantiates and keeps these instances of main systems like the _UCPP_SimulationManager, UCPP_IsolateMesh_, and now the _UQuizUIManager_ too. This means that, if any part of the application needs to access these systems, it must be done by casting to _ACPP_GameMode_.
 -  **The “God Object” Pawn**: With the current architecture, _ACPP_User_ is much more than just a pawn for player controls. It is tightly integrated with core application functionality, like managing camera controls, raycasting, and handling all input for moving and interacting with the 3D model. As the blueprint _BP_User_ (this blueprint is necessary for properly spawning the _ACPP_User_ class in levels, therefore accessing it’s functionality) is derived from _ACPP_User_, any level loaded with the _ACPP_GameMode_ automatically assumes this specific pawn will be in use.
 -	**Implicit Subsystem Dependencies**: The most critical issue is an implicit dependency on seemingly unrelated subsystems. An example of this is how _UCPP_BaseAnimation_, which is a base class for body animations, directly refers to the _SimulationManager_ from a method within the class to listen for simulation events. 
 -	**The Consequences**: When a new level is loaded with a separate _QuizGameMode_, which does not hold references to, for example, the _SimulationManager_, the application would crash, as the game mode was not initializing the _SimulationManager_. But other parts of the program, including instances of the animations, were still loaded. Methods within these animation classes would attempt to access the _SimulationManager_ (which does not exist, as it is not being initialized), receive a null pointer, and, due to improper error handling within the existing application, cause the engine to crash. 
 -	The ideal solution to this problem would be a full architectural refactoring, which would involve decoupling of the _SimulationManager, MeshSelector_, and other systems from the _GameMode_ and _User_ classes by making use of a better event bus, dependency injection, or fetching them as separate engine subsystems.

For the quizzing system backend there are separate suggestions in the [Quizzing System Backend Repo](https://github.com/jrcz-data-science-lab/virtual_anatomy_quiz_ts)

As for next features of the quizzing system, just listen to what the clients (nursing teachers) want, and go off that.

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
