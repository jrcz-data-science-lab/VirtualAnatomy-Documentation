# `UCPP_SettingsSlider`

## Class Description

**UCPP_SettingsSlider**: 

This class represents a slider in the settings page of the application. Unlike traditional sliders that allow for a full range of values, this slider provides predefined steps for the user to choose from. This design is intended to prevent excessive steps that may confuse the user and streamline the process of adjusting settings. The slider is used to modify specific settings related to the user interface and controls, such as camera sensitivity.

## Public Properties

### `TObjectPtr<USlider> MainSlider`

**Description**: 

The main slider widget that allows the user to adjust a setting. The user can select from predefined steps instead of freely adjusting the value within a continuous range.

---

### `ESensitivityOf SensitivityOf`

**Description**: 

This enum specifies which aspect of sensitivity the slider controls (e.g., camera movement sensitivity or zoom sensitivity). It helps define the specific setting that the slider will modify.

---

## Public Methods

### `void OnSliderMouseReleaseHandle()`

**Description**: 

This function handles the event when the user releases the mouse after adjusting the slider. It is used to update the setting based on the selected value.

---

## Protected Methods

### `virtual void NativeConstruct() override`

**Description**: 

Called after the widget is constructed. This function is used to perform any further setup after the widget is initialized and ready to be displayed.

---

### `virtual void NativePreConstruct() override`

**Description**: 

Called before the widget is constructed. This function can be used for any setup that needs to occur before the widget is fully created and displayed.

---

## Private Members

### `UCPP_GameInstance* GameInstanceRef`

**Description**: 

A reference to the `UCPP_GameInstance` class, which manages global game settings. This is used to retrieve or modify the settings that the slider controls.
