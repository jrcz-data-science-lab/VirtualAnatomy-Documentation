# UStudyYearSelectorWidget

* **File**: `VirtualAnatomy/Public/Widgets/Quiz/CPP_StudyYearSelectorWidget.h` & `VirtualAnatomy/Private/Widgets/Quiz/CPP_StudyYearSelectorWidget.cpp`
* **Purpose**: This widget allows the student to select their current study year and communicates this selection to the `UQuizUIManager` to filter and tailor quizzes.

## Public Properties

### `FOnStudyYearSelectedSignature OnStudyYearSelectedEvent`

**Type:** `FOnStudyYearSelectedSignature` (Delegate)

Delegate broadcasted when a study year button is clicked and a year is selected.

---

### `UQuizUIManager* QuizUIManagerRef`

**Type:** `UQuizUIManager*`

An optional direct reference to the QuizUIManager. It can be set by the widget creating this selector or found via the GameMode.

---

### `UButton* Year1Button`

**Type:** `UButton*`

Button for selecting Year 1.

---

### `UButton* Year2Button`

**Type:** `UButton*`

Button for selecting Year 2.

---

### `UButton* Year3Button`

**Type:** `UButton*`

Button for selecting Year 3.

---

### `UButton* Year4Button`

**Type:** `UButton*`

Button for selecting Year 4.

## Protected Methods

### `void NativeConstruct()`

Called when the widget is constructed. It is used to find the `QuizUIManager` and bind button click events.

## Private Methods

---

### `void OnYear1ButtonClicked()`

Handles the click event for the Year 1 button.

---

### `void OnYear2ButtonClicked()`

Handles the click event for the Year 2 button.

---

### `void OnYear3ButtonClicked()`

Handles the click event for the Year 3 button.

---

### `void OnYear4ButtonClicked()`

Handles the click event for the Year 4 button.

---

### `void HandleYearSelection(int32 Year)`

**Parameters:**

-   `int32 Year` - The selected study year.

Common logic executed when any year selection button is clicked. It notifies the `QuizUIManager` and then removes this widget from view.
