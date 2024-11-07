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
 ├── Blueprints
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
│   ├── Body-Merged-Full
│   │   ├── Animations
│   │   │   ├── Body
│   │   │   ├── Hearth
│   │   │   └── Lungs
│   │   ├── Arteries
│   │   ├── Bones
│   │   ├── Hearth
│   │   │   ├── Hearth
│   │   │   ├── PapillaryMuscle
│   │   │   └── Valves
│   │   └── Lungs
│   ├── Body-No-Material
│   │   ├── Arteries
│   │   ├── Bones
│   │   ├── Hearth
│   │   └── Lungs
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
    ├── Islands
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

This folder should ideally be renamed to `Assets` instead of `Materials`, but for now, it remains as is.

In short, this folder contains all the meshes, textures, materials, and HDRIs used in the levels.

### Materials/Body-Merged-Full

This subfolder contains all the merged body parts such as the heart, bones, and lungs. Additionally, you will also find the `Animations` folder here, which, as the name suggests, contains all the animations.

### Materials/Body-No-Materials

This subfolder contains individual meshes used for selecting specific parts of the body.


> **NOTE**: When you change an asset here, it will not be updated inside the View Port and World Explorer.

### Python

Plain and simple, here are all of the Python scripts used **only inside the engine**.

### Steredo3D_VitalVolkov

This is from developers before us and contains the ability to see the model in 3D (yes, with 3D glasses) only if your monitor supports it. This feature has not been used so far, so you can experiment with it.

### Test

This is pretty self-explanatory.

### UI

Here you can find all of the widget Blueprints. In other words, every UI component, like buttons, menus, main pages, etc., can be found and edited here.

### Virtual anatomy 

In this directory is the main source code of the entire application. Here you can find another subdirectory, `C++`, which contains the actual C++ code.

**Public**

Here are all of the header files containing definitions of classes, methods, and macros.

**Private**

This directory contains all of the definitions for the header files (.cpp).

Some header files (and cpp files) are not visible through the Unreal Editor interface (like `CPP_RayCaster`, `CPP_ObjectSelector`), but some are, like `CPP_User`. This is because the class `CPP_RayCaster` does not inherit from any of the Engine's classes like `UObject` or `AActor`. To see those "hidden" files, please open the file in Visual Studio, JetBrains Rider, VSCode, or any other IDE or code editor you prefer.

## How to Approach This Giant Project

This project is large and can feel overwhelming to grasp all at once. We’ve tried to make as many components as stable and unchanging as possible. This means that most of what you see on the screen doesn’t require a deep understanding right away—working code is best left untouched, right? 😄

For the first 2–3 weeks of your internship, we strongly encourage you to focus on learning C++, especially general OOP concepts and how pointers work. Try building a simple calculator or a rock-paper-scissors app using OOP and pointers to get comfortable.

Next, dive into general Unreal Engine tutorials. Remember, everything in Unreal Engine is highly decoupled—it’s not like a website or other applications where you can easily call everything through dependency injections or setters. We also recommend following some C++ tutorials in Unreal Engine and creating a small project to test things out. Once you get comfortable with the C++ side, using Blueprints will feel like a helpful tool for tying your logic together.

Keep in mind that C++, OOP, and other programming patterns aren’t quite the same when developing in Unreal Engine. Also, documentation can be extremely frustrating to navigate. Because of this, we strongly advise using JetBrains Rider IDE if your RAM allows it, as it provides the best autocompletion and function documentation support. If it doesn’t, try increasing your swap file to use your SSD as temporary RAM.

While this documentation isn’t perfect (we only had six months to develop, test, design, and write it!), we’ve tried to document as much as possible to help guide you through.

## Why We Used mostly C++ Instead of Blueprints

In short, for maintainability. Yes, C++ can be hard, complicated, and unforgiving, but once you become familiar with its inner workings, it will become both your best friend and, sometimes, your worst enemy. If you’re new to C++, take it slowly. If you’re not sure how to implement something, try building it in Blueprints first, even if it turns into a litteral of spaghetti code. When you reach that point, it’s usually a good time to start rewriting it in C++.




