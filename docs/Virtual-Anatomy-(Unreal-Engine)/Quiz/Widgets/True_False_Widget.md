# UCPP_TrueFalseWidget

* **File**: `VirtualAnatomy/Public/Widgets/Quiz/Questions/CPP_TrueFalseWidget.h` & `VirtualAnatomy/Private/Widgets/Quiz/Questions/CPP_TrueFalseWidget.cpp`
* **Purpose**: Displays true/false questions to the user.

## Public Methods

### `void InitializeWithQuestion(const FQuizQuestion& Question)`

**Parameters:**

-   `const FQuizQuestion& Question` - The quiz question data to initialize the widget with.

Initializes the widget with the provided quiz question, setting the question text and resetting `bLastSelectedTrue`.

---

### `bool bLastSelectedTrue`

**Type:** `bool`
**Default Value:** `false`

Indicates if the "True" option was the last one selected by the user.

## Protected Methods

### `void NativeConstruct()`

Called when the widget is constructed. It binds click events for the True and False buttons.

---

### `UTextBlock* QuestionText`

**Type:** `UTextBlock*`

The text block widget used to display the question text.

---

### `UButton* TrueButton`

**Type:** `UButton*`

The button widget for the "True" option.

---

### `UButton* FalseButton`

**Type:** `UButton*`

The button widget for the "False" option.

## Protected Methods

### `void OnTrueClicked()`

Handles the click event for the True button, calling `HandleAnswer(true)`.

---

### `void OnFalseClicked()`

Handles the click event for the False button, calling `HandleAnswer(false)`.

---

### `void HandleAnswer(bool bSelectedTrue)`

**Parameters:**

-   `bool bSelectedTrue` - `true` if "True" was selected, `false` if "False" was selected.

Processes the selected answer, sets `bLastSelectedTrue`, and broadcasts the `OnQuestionAnswered` delegate.
