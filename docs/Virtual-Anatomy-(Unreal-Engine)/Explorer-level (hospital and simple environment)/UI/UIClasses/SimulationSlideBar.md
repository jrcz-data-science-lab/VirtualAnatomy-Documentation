# `UCPP_SimulationSlideBar`

## Class Description

**UCPP_SimulationSlideBar**: 

This widget is responsible for creating and managing sliders that allow the user to control simulation parameters, such as speed and other user-defined properties. The slider adjusts global simulation parameters and sends updates to the simulation flow. It provides a user-friendly interface to control the flow of the simulation in real-time.

## Public Properties

### `TObjectPtr<UTextBlock> SliderHeader`

**Description**: 

A text block that displays the header for the slider, typically showing what parameter the slider controls.

---

### `FString SliderHeaderValue`

**Description**: 

The value of the header text. This is a bindable property to allow dynamic changes to the header text.

---

### `TObjectPtr<UTextBlock> MaxValueTextBlock`

**Description**: 

A text block that displays the maximum value for the slider. It is used to give the user context about the range of values the slider can take.

---

### `FString MaxValue`

**Description**: 

A string that represents the maximum value the slider can have. This value is used to display the maximum limit to the user.

---

### `TObjectPtr<UTextBlock> MinValueTextBlock`

**Description**: 

A text block that displays the minimum value for the slider. It is used to give the user context about the range of values the slider can take.

---

### `FString MinValue`

**Description**: 

A string that represents the minimum value the slider can have. This value is used to display the minimum limit to the user.

---

### `TObjectPtr<USlider> MainSlider`

**Description**: 

The main slider widget that allows the user to interact with the parameter values. It is used to adjust the value of the specified parameter.

---

### `TObjectPtr<UImage> TooltipImage`

**Description**: 

An image widget that provides a tooltip or visual indication for the slider. It helps the user understand what the slider controls.

---

### `FString ToolTipValue`

**Description**: 

A string that represents the tooltip text associated with the slider. This is displayed to help guide the user about the function of the slider.

---

### `ESimulationParamToChange ParameterToChange`

**Description**: 

This enum specifies what parameter inside the `FSimulationSlideBarsParameters` struct will be changed by this slider. It allows the slider to affect different properties, such as speed or scale factor, depending on the enum selection.

---

## Protected Methods

### `virtual void NativePreConstruct() override`

**Description**: 

Called before the widget is constructed. This function can be used for any initial setup before the widget is displayed.

---

### `virtual void NativeConstruct() override`

**Description**: 

Called after the widget has been constructed. This function is typically used for further setup or initialization that requires the widget to be fully created.

---

### `UFUNCTION() void HandleSliderRelease()`

**Description**: 

Handles the slider's value release event. It is triggered when the user releases the slider after adjusting its value.

---

## Private Members

### `TObjectPtr<UCPP_SimulationManager> SimulationManager`

**Description**: 

A pointer to the `UCPP_SimulationManager` that manages the simulation. It is used to update the simulation parameters based on the slider's value.
