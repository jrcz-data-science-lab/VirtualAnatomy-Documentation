# First glance at the project

You have now entered the `Explorer Level` of the project and are probably wondering where shoul I even start. No worries we got you covered. 

Scene you see loaded is in the `View port` this is where you can interact with different `Actors`, be it static mesh, post proecessing volume and more. 

On the very right side you see your `World Explorer` this is everything you see in the `View Port` organized in folders. Each folder contains one or more `Scene elements` each scene element contains one or more `Actors` that are displayed in your view port, assuming they have something to display like `static mesh`. 

## What is in the scene 

### Folders with names of human body
Those folders contains exactly what you might immagine. Scene componets with the `static meshes` inside them. Each scene component represents part of the model.

For example in `Bones` directory you can find `Skull` sub-directory and inside it lot of scene compoents that are staric meshes which contain actual geomtery and material of fragment of human skull. Try to click on one to see where in the `View Port` is it located. 


## Contetn browser

On the very bottom of the unreal engine you shoudl see Icon with folder this is called `Content browser`. Here all of the project files, assets and `bluerprints` are located. This is what you can find there.


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


That is lot of directories right ? Lets walk through them really supperficialy to get basic idea of what is going on.

### Blueprints

Here all of the blueprints are located you can find here blueprints for User that can walk around (inside `UserControls` folder) in the scene once the simulation is started (we will talk about this later)

### Developoers

Here each developer can store its personalized assets, nothing should be here unless you want to hide something from your colleagues 

### Enums

Contain enums to deferntiate between parts of the model like skull, muslces, hearth etc...

### Input

Here are inputs and input mappings from utilising [Unreal Engine Enhanced Input system](https://dev.epicgames.com/documentation/en-us/unreal-engine/enhanced-input-in-unreal-engine)

In short here are defined all user actions. Simple example of such action is press `Space` to Jump

### Maps

Here we store different `levels` that user can visit during his time spend in application.

Levels are independant of each other and they are exactly like levels in any video game

### Materials

Here are stored all the `Assets` this should be renamed to `Assets` instead of materials, but be it.

In short one can find here all of the meshes, textures, materials and HDRIs that are used in levels. 

> **NOTE**: When you change asset here it will not be updated inside the view port and world explorer

### Python

Plain and simple as it gets here are all of the python scripts used **only inside the engine**

### Steredo3D_VitalVolkov

This is from developers before us, and it contains ability to see the model in 3D (yes, with 3D glasses) onyl if your monitor supports it. This feature was not used so far so you can expremint with it.


### Test 

This is pretty self explenatory. 

### UI

Here you can find all of the widgets blueprints. In other words every UI componet like button, menu, main page etc... can be found and edited here 

### EndlessRunner 

>FUN FACT: Previous develoopers initialy named project Endlesss runner for some reason and it became quite difficult to rename it after they were half way through the development, this resulted that now this project is called EndlessRunnder, feel free to rename it. Good luck 

In this directly is the main source code of the entire application. Here you can find another sub directory `C++` which is the acctual C++ code.

**Public**

Here are all of the header files containg deffinitions of class, methods and macros. 

**Private**

This directory contains all of the deffinitons of the header files (.cpp)


Some of the header files (and cpp files) are not visible through the Unreal Edditor interface (like `CPP_RayCaster`, `CPP_ObjectSelector`) but some are like `CPP_User`. This is because class `CPP_RayCaster` does not inherit any of the Engine's class like `UObject or AActor` to see those "hidden" files please open the file in Visual Studio, JetBrains Rider or VSCode, or any other IDE / code editor you want. 
