# `UCPP_SlicerSlideBar`

This is a custom UMG widget designed to serve as a parameter control component in the slicer sidebar interface. It displays a labeled slider with configurable minimum and maximum values, a default value, a tooltip, and a visual range. The widget is tightly integrated with the `UCPP_SlicerSideBar` class and serves to control either the slicer's distance or rotation, depending on its assigned purpose.

This component automatically updates its visual text fields (like min/max values and header) based on editable `UPROPERTY` values and reacts to user input in real-time. Whenever the slider value changes, it invokes a delegate-style callback which is handled externally - typically by the sidebar UI - to propagate the new value to the slicer actor.

## Public Properties

### `TObjectPtr<USlider> MainSlider`  

**Description:**

The primary slider widget for user interaction.

### `TObjectPtr<UTextBlock> SliderHeader`  

**Description:**

  Displays the label/title of the slider.

### `TObjectPtr<UTextBlock> MaxValueTextBlock`  

**Description:**

  Displays the textual representation of the maximum value.

### `TObjectPtr<UTextBlock> MinValueTextBlock`  

**Description:**

  Displays the textual representation of the minimum value.

### `FText SliderHeaderValue`  

**Description:**

  Default label value for the slider header (set in Editor).

### `FString MaxValue`  

**Description:**

  Maximum value label shown next to the slider.

### `FString MinValue`  

**Description:**

  Minimum value label shown next to the slider.

### `float MaxValueForSlider`  

**Description:**

  The maximum numeric value the slider can reach.

### `float MinValueForSlider`  

**Description:**

  The minimum numeric value the slider can reach.

### `float StepSize`  

**Description:**

  Incremental step the slider moves in.

### `float DefaultValue`  

**Description:**

  Default value the slider starts at when constructed.

### `FText ToolTipValue`  

**Description:**

  Tooltip text shown when hovering over the slider.

### `ESlicerParamToChange ParameterToChange`  

**Description:**

  Enum defining which parameter this slider controls (e.g., distance or rotation).

### `TFunction<void(float)> OnSliderValueChangedCallback`  

**Description:**

  A delegate-style function pointer. Called when the slider value changes. Set externally by `UCPP_SlicerSideBar`.z

## Protected Methods

### `void OnSliderValueChanged(float NewValue)`

**Description:**

Callback function bound to the slider. Called internally when the user changes the slider value. It triggers the `OnSliderValueChangedCallback` to notify external components like the slicer sidebar.

**Parameters:**
- `float NewValue`: The new value of the slider after user interaction.

## Usage

`UCPP_SlicerSlideBar` is typically used within `UCPP_SlicerSideBar` to allow user control over slicer parameters. When a user adjusts the slider, the class emits the new value via the bound callback, and this value is applied to the slicing logic in the `ACPP_Slicer` actor.