---
weight: -1
---

# Content browser

On the very bottom of the Unreal Engine, you should see an icon with a folder; this is called the `Content Browser`. Here, all the project files, assets, and Blueprints for the Virtual Anatomy are located.

You can see below the content browser structure of this Unreal Engine project. There is a lot, but you do not need to swot it all at once. You will get used to it as you work with this project for a long time. 

```shell
.
├── Blueprints
│   ├── BloodFlowSimulation
│   │   └── BloodSimPaths
│   ├── Enviroment
│   ├── InfoElements
│   ├── Quiz
│   ├── Splines
│   └── UserControls
├── Collections
├── DataAssets
├── Developers
│   └── bartoszadamczyk
│       └── Collections
├── Input
│   └── InputActions
├── Localization
│   └── Game
│       ├── en
│       └── nl-NL
├── LocalizationDataTables
├── Maps
├── Models
│   ├── Body-Merged-Full
│   │   ├── Arteries
│   │   ├── BodyPartBlueprints
│   │   │   ├── Body
│   │   │   ├── Digestion
│   │   │   ├── Hearth
│   │   │   └── Lungs
│   │   ├── Bones
│   │   ├── Digestive_System
│   │   ├── Heart
│   │   │   ├── Heart
│   │   │   ├── PapillaryMuscle
│   │   │   ├── SubcalvinianArteries
│   │   │   └── Valves
│   │   ├── Lungs
│   │   ├── Muscles
│   │   └── Veins
│   ├── Body-No-Material
│   │   ├── Arteries
│   │   ├── Bones
│   │   ├── Digestive_System
│   │   ├── Hearth
│   │   └── Lungs
│   ├── Body-Picker-Textured
│   │   └── Arteries
│   │       └── Components
│   ├── ElementActions
│   │   └── Outliner
│   ├── Enviroment
│   │   ├── BookShelf
│   │   ├── HopitalRoomFloor
│   │   │   └── textures
│   │   ├── HospitalBed
│   │   ├── HospitalDoors
│   │   ├── HospitalEquipment
│   │   ├── HospitalRoom-Wall
│   │   │   ├── textures_new
│   │   │   └── textures_old
│   │   ├── HospitalRoomAssetPack
│   │   ├── IndoorPlant
│   │   └── NeonLight
│   ├── Full-Body-Organized
│   │   ├── Digestive_System
│   │   ├── Ear_Bones
│   │   ├── Lower_Extremity_Bones
│   │   │   └── Lower_Extremity_Bones
│   │   ├── Skull_Bones
│   │   ├── Thoracic_Bones
│   │   │   └── Textures
│   │   ├── Upper_Extremity_Bones
│   │   └── Vertebral_Column_Bones
│   ├── HDRi
│   ├── HospitalRoomAssetPack
│   └── Model
│       ├── Textures
│       │   └── PulmonaryVeins
│       └── Window
├── Movies
├── Niagara
├── Python
│   ├── DataExports
│   └── GeneratedSeedData
├── Sequences
│   ├── BloodPaths
│   └── Misc
├── Slicer
└── UI
    ├── Assets
    │   ├── ButtonAssets
    │   └── General
    ├── ButtonsForShocks
    ├── Components
    │   ├── CameraControls
    │   ├── Depricated
    │   ├── DropDownMenues
    │   ├── PopUps
    │   ├── SideBar
    │   │   └── TreeView
    │   ├── Sliders
    │   └── ToolTips
    ├── Fonts
    ├── Icons
    │   ├── Element
    │   ├── Mouse
    │   └── Sidebar
    ├── Images
    ├── Islands
    ├── LevelUI
    │   ├── ExplorerLevelUI
    │   └── QuizUI
    ├── Menus
    ├── Quiz
    │   └── Questions
    └── ReusableButtons
```

That is a lot of directories, right? Let's walk through them really superficially to get a basic idea of what is going on.

## Blueprints

Here, all of the Blueprints are located. You can find the Blueprints for the User that can walk around (inside the `UserControls` folder) in the scene once the simulation is started (we will talk about this later).

## Developers

Here, each developer can store their personalized assets. Nothing should be here unless you want to hide something from your colleagues.

## Enums

This folder contains Enums to differentiate between parts of the model, like the skull, muscles, heart, etc.

## Input

This directory contains inputs and input mappings utilizing the [Unreal Engine Enhanced Input system](https://dev.epicgames.com/documentation/en-us/unreal-engine/enhanced-input-in-unreal-engine).

In short, all user actions are defined here. A simple example of such an action is pressing `Space` to jump.

## Maps

Here we store different levels that users can visit during their time spent in the application.

Levels are independent of each other and are exactly like levels in any video game.

## Materials

This folder should ideally be renamed to `Assets` instead of `Materials`, but for now, it remains as is.

In short, this folder contains all the meshes, textures, materials, and HDRIs used in the levels.

## Materials/Body-Merged-Full

This subfolder contains all the merged body parts such as the heart, bones, and lungs. Additionally, you will also find the `Animations` folder here, which, as the name suggests, contains all the animations.

## Materials/Body-No-Materials

This subfolder contains individual meshes used for selecting specific parts of the body.


> **NOTE**: When you change an asset here, it will not be updated inside the View Port and World Explorer.

## Python

Plain and simple, here are all of the Python scripts used **only inside the engine**.

## Steredo3D_VitalVolkov

This is from developers before us and contains the ability to see the model in 3D (yes, with 3D glasses) only if your monitor supports it. This feature has not been used so far, so you can experiment with it.

## Test

This is pretty self-explanatory.

## UI

Here you can find all of the widget Blueprints. In other words, every UI component, like buttons, menus, main pages, etc., can be found and edited here.

## Virtual anatomy

In this directory is the main source code of the entire application. Here you can find another subdirectory, `C++`, which contains the actual C++ code.

**Public**

Here are all of the header files containing definitions of classes, methods, and macros.

**Private**

This directory contains all of the definitions for the header files (.cpp).

Some header files (and cpp files) are not visible through the Unreal Editor interface (like `CPP_RayCaster`, `CPP_ObjectSelector`), but some are, like `CPP_User`. This is because the class `CPP_RayCaster` does not inherit from any of the Engine's classes like `UObject` or `AActor`. To see those "hidden" files, please open the file in Visual Studio, JetBrains Rider, VSCode, or any other IDE or code editor you prefer.




