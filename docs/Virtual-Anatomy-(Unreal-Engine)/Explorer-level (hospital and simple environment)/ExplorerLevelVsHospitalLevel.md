---
weight: -1
---

# Simple environment vs. Hospital environment

You might be wondering what the difference is between the simple environment and the hospital environment. Let us break it down:

## Hospital Environment
(named **ExplorerLevel-HospitalEnvironment** as a level inside the Unreal Engine project)

The **Hospital Level** is designed primarily for presenting the application to clients. As such, it includes advanced and resource-intensive features like:

- Expensive lighting operations
- Real-time ray tracing
- Global illumination
- Light occlusion
- Lens flare
- God rays

While these features make the level visually stunning, they also make it extremely demanding on system resources. For this reason, we strongly discourage working on this level. It is not that we do not want you to have fun — it is just that this level is a resource-hungry powerhouse best reserved for client presentations.

## Simple Environment 
(named **ExplorerLevel-SimpleEnvironment** as a level inside the Unreal Engine project)

In contrast, the Simple Environment is designed for speed and performance. All new features should be implemented in this level, as performance is critical for this project. It’s optimized to ensure smooth operation and efficiency, making it the ideal environment for development and testing. Once new features are implemented you are free to put and test them to the hospital level. 

## Cinematic Sequences
You might also be curious about the purpose of the cinematic sequences. As the name suggests, these are used to create the application’s trailer. The master sequencer governs every shot, which is essentially a prerecorded camera movement. These sequences help showcase the application in a dynamic and engaging way.
