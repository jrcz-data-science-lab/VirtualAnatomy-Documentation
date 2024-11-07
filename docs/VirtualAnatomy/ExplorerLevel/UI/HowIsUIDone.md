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

### Bindings 

In some of the header files of user widgets, you might see code similar to this one 

```c++
		UPROPERTY(BlueprintReadWrite, meta = (BindWidget))
		TObjectPtr<UHorizontalBox> HorizontalFlexBox;
	
		UPROPERTY(BlueprintReadWrite, meta = (BindWidget))
		TObjectPtr<UBorder> Background;
```
This simply defines that the UI created based on this class will include these components. If you look inside the widget blueprint for this class, you might see these two components listed in the Binding tab of the UMG editor.


### Simple example

- Create the Widget: Open the UMG editor and create a new widget named MyCustomWidget.
- Add Components: Drag a Button and a TextBlock onto the canvas.
- Match Names: Rename the Button to MyButton and the TextBlock to MyTextBlock to match the names defined in your class.

> **IMPORTANT**: Names of the class property and widget in the UMG editor MUST match otherwise you will get compile errors 

By following these steps, the UMG editor will automatically bind these components to the properties in your class, allowing you to manipulate them in your Blueprint or C++ code.

This approach was chosen to enable us to change the properties of specific UI elements directly from the C++ class without any issues.
