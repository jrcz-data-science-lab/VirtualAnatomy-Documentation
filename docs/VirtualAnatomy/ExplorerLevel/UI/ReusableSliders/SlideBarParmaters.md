# SlideBarsParameters.h

## Overview
This file defines the structures and enumerations used for controlling global simulation parameters and camera sensitivity settings via sliders. It includes functions for manipulating the simulation speed and scale, as well as adjusting camera movement and zoom sensitivities.

---

## Enumerations

### ESimulationParamToChange
An enumeration for the parameters that can be changed via sliders in the simulation.

- **Speed**: Represents the speed of the simulation.
- **ScaleFactor**: Represents the scaling factor that affects the speed of the simulation.

### ESensitivityOf
An enumeration for different types of camera sensitivity settings.

- **CAMERA_MOVEMENT**: Represents the sensitivity of camera movement.
- **CAMERA_ZOOM**: Represents the sensitivity of camera zoom.

---

## Structs

### FSimulationSlideBarsParameters
Represents the global parameters of the simulation that are controlled by sliders.

#### Members:
- **float Speed**: The speed of the simulation, determining how long it takes the cell to complete one full cycle.
- **float ScaleFactor**: A scaling factor applied to adjust the simulation speed, controlled by a slider.
  
#### Functions:
- **float GetSimulationSpeed() const**: Returns the actual speed of the simulation, calculated as `Speed * ScaleFactor`.
  
- **float& GetFieldToChangeBySlider(ESimulationParamToChange paramToChange)**: Returns a reference to the field of the struct that the slider should change. The parameter `paramToChange` determines whether the slider will adjust `Speed` or `ScaleFactor`.

---

### FCameraSensitivitySettings
Represents the sensitivity settings for the camera, including movement and zoom sensitivity.

#### Members:
- **float CameraMovementSensitivity**: Default value for camera movement sensitivity (default: 1000.0f).
- **float CameraZoomSensitivity**: Default value for camera zoom sensitivity (default: 1600.0f).
- **float SliderValueForCameraMovement**: Slider value for camera movement sensitivity (default: 1.0f).
- **float SliderValueForCameraZoom**: Slider value for camera zoom sensitivity (default: 1.0f).

#### Functions:
- **float& GetSensitivityOf(ESensitivityOf sensitivityOf)**: Returns a reference to the specified sensitivity setting. The `sensitivityOf` parameter can be `CAMERA_MOVEMENT` or `CAMERA_ZOOM`.
  
- **float& GetSliderValueFor(ESensitivityOf sensitivityOf)**: Returns a reference to the slider value for the specified sensitivity setting. The `sensitivityOf` parameter can be `CAMERA_MOVEMENT` or `CAMERA_ZOOM`.

---

## Error Handling
- In `GetFieldToChangeBySlider` and `GetSensitivityOf`, if an invalid parameter is passed (i.e., an unknown enum value), an error message is logged using `UE_LOG`.
