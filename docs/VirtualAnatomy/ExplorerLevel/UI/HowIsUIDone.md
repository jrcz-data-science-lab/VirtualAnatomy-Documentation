## How the UI is Done

In this section, we will discuss and explain how we created the UI.

### General Approach

The overall approach was to create a `C++` class to represent the logic of each UI element. These classes inherit from the `UUserWidget` class provided by the Unreal Engine. All the widget-related classes are stored inside the `Widgets` folder within the `Source` directory.

Once the widget class is created, we generate a `Widget Blueprint` that inherits from the desired C++ class.

### Example: Creating a Button

Let's say we want to create a button named `UCPP_FooButton`. Here's the process we would follow:

1. **Create the C++ Class**  
   First, we create a `C++` class and make it inherit from `UUserWidget`. 
   
2. **Organize the Folder Structure**  
   We either specify a new folder inside the `Widgets` folder or use an already existing one. This helps keep things organized.
   
3. **Create the Widget Blueprint**  
   After the class is created, we navigate to the `UI/Widgets` folder where we create a `Widget Blueprint`. This blueprint will have the C++ class (`UCPP_FooButton`) as its parent.

### Why This Approach?

We chose this method to write clean and clear code in `C++` while allowing easy access to functions via blueprints. Additionally, it lets us design various UI components using the Unreal Engine's UI editor, combining the flexibility of C++ logic with the simplicity of the visual UI editor.
