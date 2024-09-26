# First Glance at the Project

You have now entered the `Explorer Level` of the project and are probably wondering where you should even start. No worries, we’ve got you covered.

The scene you see loaded is in the `View Port`. This is where you can interact with different `Actors`, be it static meshes, post-processing volumes, and more.

On the very right side, you see your `World Explorer`. This contains everything you see in the `View Port`, organized in folders. Each folder contains one or more `Scene Elements`, and each scene element contains one or more `Actors` that are displayed in your View Port, assuming they have something to display, like a `Static Mesh`.

## What Is in the Scene

### Folders with Names of Human Body

Those folders contain exactly what you might imagine: scene components with the `Static Meshes` inside them. Each scene component represents a part of the model.

For example, in the `Bones` directory, you can find a `Skull` sub-directory, and inside it, there are a lot of scene components that are static meshes, which contain the actual geometry and material of fragments of the human skull. Try to click on one to see where it is located in the `View Port`.

## Content Browser

On the very bottom of the Unreal Engine, you should see an icon with a folder; this is called the `Content Browser`. Here, all of the project files, assets, and `Blueprints` are located. This is what you can find there.


```shell
 Blueprints
│   ├── InfoElements
│   └── UserControls
├── Collections
├── Developers
│   └── wpsimon09
│       └── Collections
├── Enums
├── Input
│   └── InputActions
├── Localization
│   └── Game
│       ├── en
│       └── nl-NL
├── Maps
├── Materials
│   ├── ElementActions
│   │   ├── Outliner
│   │   └── Slicer
│   ├── Full-Body-Organized
│   │   ├── Ear_Bones
│   │   ├── Lower_Extremity_Bones
│   │   │   └── Lower_Extremity_Bones
│   │   ├── Skull_Bones
│   │   ├── Thoracic_Bones
│   │   │   └── Textures
│   │   ├── Upper_Extremity_Bones
│   │   └── Vertebral_Column_Bones
│   ├── Full-body-test-all-in-one
│   ├── HDRi
│   ├── Model
│   │   ├── Materials
│   │   │   ├── Heart
│   │   │   │   ├── ConductionSystem
│   │   │   │   ├── CoronaryVessels
│   │   │   │   ├── EpicardialFat
│   │   │   │   ├── Misc
│   │   │   │   ├── Muscle
│   │   │   │   ├── OutflowTract
│   │   │   │   └── PapillaryMuscle
│   │   │   ├── Lungs
│   │   │   ├── PulmonaryArteries
│   │   │   └── PulmonaryVeins
│   │   ├── StaticMesh
│   │   │   ├── Heart
│   │   │   │   ├── ConductionSystem
│   │   │   │   ├── CoronaryVessels
│   │   │   │   ├── EpicardialFat
│   │   │   │   ├── Misc
│   │   │   │   ├── Muscle
│   │   │   │   ├── OutflowTract
│   │   │   │   ├── PapillaryMuscle
│   │   │   │   └── Valves
│   │   │   ├── Lungs
│   │   │   ├── PulmonaryArteries
│   │   │   └── PulmonaryVeins
│   │   └── Textures
│   │       ├── Heart
│   │       │   ├── CoronaryVessels
│   │       │   ├── Muscle
│   │       │   └── OutflowTract
│   │       ├── Lungs
│   │       └── PulmonaryVeins
│   └── TestSkeleton
├── Python
├── Stereo3D_VitalVolkov
│   ├── Blueprints
│   ├── Input
│   │   └── Actions
│   └── Materials
├── Tests
└── UI
    ├── Assets
    │   ├── ButtonAssets
    │   └── General
    ├── Components
    │   └── Quiz
    │       └── Questions
    ├── FloatingWindows
    ├── Fonts
    ├── Icons
    │   ├── Element
    │   ├── Mouse
    │   └── Sidebar
    ├── Images
    │   └── LocationWidget
    ├── LevelUI
    │   ├── ExplorerLevelUI
    │   │   └── Sidebars
    │   └── QuizLevelUI
    │       └── Sidebars
    ├── Menus
    └── ReusableButtons

```

That is a lot of directories, right? Let's walk through them really superficially to get a basic idea of what is going on.

### Blueprints

Here, all of the Blueprints are located. You can find the Blueprints for the User that can walk around (inside the `UserControls` folder) in the scene once the simulation is started (we will talk about this later).

### Developers

Here, each developer can store their personalized assets. Nothing should be here unless you want to hide something from your colleagues.

### Enums

This folder contains Enums to differentiate between parts of the model, like the skull, muscles, heart, etc.

### Input

This directory contains inputs and input mappings utilizing the [Unreal Engine Enhanced Input system](https://dev.epicgames.com/documentation/en-us/unreal-engine/enhanced-input-in-unreal-engine).

In short, all user actions are defined here. A simple example of such an action is pressing `Space` to jump.

### Maps

Here we store different levels that users can visit during their time spent in the application.

Levels are independent of each other and are exactly like levels in any video game.

### Materials

This folder should be renamed to `Assets` instead of `Materials`, but it is what it is.

In short, you can find here all of the meshes, textures, materials, and HDRIs that are used in the levels.

> **NOTE**: When you change an asset here, it will not be updated inside the View Port and World Explorer.

### Python

Plain and simple, here are all of the Python scripts used **only inside the engine**.

### Steredo3D_VitalVolkov

This is from developers before us and contains the ability to see the model in 3D (yes, with 3D glasses) only if your monitor supports it. This feature has not been used so far, so you can experiment with it.

### Test

This is pretty self-explanatory.

### UI

Here you can find all of the widget Blueprints. In other words, every UI component, like buttons, menus, main pages, etc., can be found and edited here.

### EndlessRunner

> **FUN FACT**: Previous developers initially named the project "Endless Runner" for some reason, and it became quite difficult to rename it after they were halfway through the development. This resulted in the project now being called "EndlessRunnder." Feel free to rename it; good luck!

In this directory is the main source code of the entire application. Here you can find another subdirectory, `C++`, which contains the actual C++ code.

**Public**

Here are all of the header files containing definitions of classes, methods, and macros.

**Private**

This directory contains all of the definitions for the header files (.cpp).

Some header files (and cpp files) are not visible through the Unreal Editor interface (like `CPP_RayCaster`, `CPP_ObjectSelector`), but some are, like `CPP_User`. This is because the class `CPP_RayCaster` does not inherit from any of the Engine's classes like `UObject` or `AActor`. To see those "hidden" files, please open the file in Visual Studio, JetBrains Rider, VSCode, or any other IDE or code editor you prefer.
