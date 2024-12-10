# Introduction  

This section explains how the framework for sliders in the application was developed, how it works, and how you can extend it.  

## Types of Sliders  

Currently, there are two types of sliders in the application:  

1. **Simulation Slider:**  
    Found in `CPP_SimulationSlideBar.h/.cpp`.  
   
    Controls simulation-related parameters.  

2. **Settings Slider:**  
    Located in `UCPP_SettingsSlider.h/.cpp`.  
    
    Used on the settings page to adjust various application settings.  

This approach ensures visually coherent sliders with different functionalities based on the application's context. We aimed to make this framework as user-friendly as possible, meaning you only need to define a few variables to create a new slider that controls a unique part of the application.  

Before explaining how to implement this, let’s cover the general structure of these reusable sliders.  

## Structure  

Both `UCPP_SimulationSlideBar` and `UCPP_SettingsSlider` inherit from `UUserWidget`, allowing for easy styling in the editor. Each slider serves as a parent class to the corresponding `WidgetBlueprint` located in `Content/UI/Components/Sliders`. The prefix `WB_` distinguishes widget blueprints from regular blueprint classes.  

In the editor, you can adjust the appearance of sliders and related components such as headers, maximum, and minimum markers.  

### Reusability  

The most important aspect of the framework is its reusability. Since sliders can affect various parameters in the simulation or application settings, these parameters can be configured directly in the editor during the design process. Image below ilustrates one of the sliders displayed on settings page and its corresponding `enum` that is specifing which fields to change.

<figure markdown="span">
  ![coresponding enum](https://jrcz-data-science-lab.github.io/VirtualAnatomy-Documentation/images/images/slider-setting-example.png)
   <figcaption>Simulation settings and correcponidng enum in the code </figcaption>
</figure>

> **Note:**  
> If you find this approach overly complex, feel free to adjust it, but be sure to update this documentation accordingly.

## Data Storage  

The actual values that are modified by the sliders are stored in a `Struct` defined in `Types/SlideBarsParameters.h`. This struct holds all the relevant slider parameters used throughout the application.  

# Settings Slider  

The settings slider adjusts application-related parameters such as camera zoom and movement sensitivity. It works by retrieving specific fields from the struct and updating them when the slider's UI element triggers the `OnMouseCaptureEnd` event.  

### Data Retrieval Process  

Since the settings slider is expected to be used primarily on the settings page, data retrieval should occur during the `OnBeginPlay` event of different levels. This is because when a user leaves the current level (e.g., to access the settings), that level gets unloaded. When the user returns, the level is reloaded, triggering the `OnBeginPlay` event again, where data can be fetched and applied.

### Considerations  

If the level is not refreshed, you need to manually trigger an event that updates parameters like camera rotation sensitivity and zoom sensitivity. This ensures that changes made in the settings are applied in real-time without requiring a level reload.

The struct for the settings slider that holds data of different parameters looks like the code listing below. In case you want to have another settings here just simply add the new field to hold the value to change. You must not forget to also add the field that is going to hold the slider state, that is the value of the slider. 



```c++

USTRUCT()
struct FCameraSensitivitySettings
{
	GENERATED_BODY()


	/**
	 * These are default values, they are changed by multiplying settings slider value based on the specified enum
	 * The settings slider is in the range 1 - 2 meaning that when slider is 1 the defaults value is applied otherwise
	 * (value * sliderValue) applies. If slider value is 2 than Sensitivity is multiplied by 2
	 */
	float CameraMovementSensitivity{1000.0f};
	float CameraZoomSensitivity{1600.0f};



	/**
	 * Slider values so that we can	save the state of the slider through the game, slider is from range
	 * 1 to 2 so MAKE SURE THAT THIS CONDITION IS MET
	 */
	float SliderValueForCameraMovement{1.0f};
	float SliderValueForCameraZoom{1.0f};
}
```

# Enum-Based Value Selection  

Another crucial component of the slider architecture is the use of an `enum`, which specifies which values inside the struct should be modified.  

### Why Use Enums?  

Since direct access to struct values in the Unreal Engine editor is not possible, we use `enums` to bridge this gap. Enums allow us to create dropdown combo boxes within the editor, making it easy to select which struct fields should be affected by a specific slider.  

### How It Works  

- **Enum Definition:**  
  The `enum` defines all possible parameters that a slider can modify.  
- **Editor Integration:**  
  The editor displays the `enum` as a dropdown list, allowing developers to choose which struct field the slider should control.  
- **Data Binding:**  
  When a slider event fires (e.g., `OnMouseCaptureEnd`), the framework checks the selected `enum` value and updates the corresponding field in the struct accordingly.  

This setup keeps the architecture flexible and user-friendly, enabling parameter binding directly from the editor without requiring additional code modifications.

```c++
UENUM(BlueprintType)
enum class ESensitivityOf :uint8
{
    // access FCameraSensitivitySettings.CameraMovementSensitivity
	CAMERA_MOVEMENT UMETA(DisplayName = "Camera movement sensitivity"),
    // access FCameraSensitivitySettings.CameraZoomSensitivity
	CAMERA_ZOOM UMETA(DisplayName = "Camera zoom sensitivity"),
};
``` 


# Enum-Driven Field Access  

Based on the specified `enum` type selected in the editor, the corresponding struct field is retrieved and modified by the slider. This functionality is implemented as a method inside the relevant struct.  

### Example: Camera Sensitivity Settings  

For instance, the `FCameraSensitivitySettings` struct includes a function that uses the provided `enum` value to return a reference to the appropriate field within the struct. This allows the slider to modify the correct parameter dynamically.  

### How It Works  

1. **Enum Selection:**  
    In the editor, the developer selects an `enum` value corresponding to a specific field in the struct.  

2. **Field Retrieval Method:**  
    The selected `enum` is passed to a method within the struct.  
   
    The method returns a reference to the relevant field based on the `enum` value.  

3. **Value Update:**  
    When the slider fires its value change event, the retrieved struct field is updated accordingly.  

By centralizing this logic within the struct, the framework maintains clean and scalable code while supporting easy parameter binding from the editor.


```c++

	float& GetSensitivityOf(ESensitivityOf sensitivityOf)
	{
		switch (sensitivityOf)
		{
		case ESensitivityOf::CAMERA_MOVEMENT:
			return CameraMovementSensitivity;
		case ESensitivityOf::CAMERA_ZOOM:
			return CameraZoomSensitivity;
		default:
			UE_LOG(LogTemp, Error, TEXT("Unknown sensitivity settings, please check if your value is in ESensitivityOfEnum !"))
			return CameraMovementSensitivity;
		}
	}
```

Both structs are instantiaded inside the `CPP_GameInstance.cpp` file in the constructor through initialization list. With this approach values of the  struct stay presistant through the entire runtime of the application. Note that this applies onli for the Settigns structs.

<figure markdown="span">
  ![coresponding enum](https://jrcz-data-science-lab.github.io/VirtualAnatomy-Documentation/images/images/SliderArchitecture-Page-2.png)
   <figcaption>Simulation settings and correcponidng enum in the code </figcaption>
</figure>


