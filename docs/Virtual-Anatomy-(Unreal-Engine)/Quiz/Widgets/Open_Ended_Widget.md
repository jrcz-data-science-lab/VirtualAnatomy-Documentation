# UCPP_OpenEndedWidget

* **File**: `VirtualAnatomy/Public/Widgets/Quiz/Questions/CPP_OpenEndedWidget.h` & `VirtualAnatomy/Private/Widgets/Quiz/Questions/CPP_OpenEndedWidget.cpp`
* **Purpose**: Displays open-ended questions where the user types a response.

## Public Methods

### `void NativeConstruct()`

Called when the widget is constructed. It binds the `OnClicked` event to the `SubmitButton`.

---

### `void InitializeWithQuestion(const FQuizQuestion& Question)`

**Parameters:**

-   `const FQuizQuestion& Question` - The quiz question data to initialize the widget with.

Initializes the widget with the provided quiz question, setting the question text and clearing any previous answer input.

---

### `FString SubmittedAnswer`

**Type:** `FString`

The string containing the user's submitted answer.

## Protected Methods

### `void OnSubmitClicked()`

Handles the click event for the Submit button, capturing the text from the `AnswerInput` and broadcasting the `OnQuestionAnswered` delegate.

---

### `UTextBlock* QuestionText`

**Type:** `UTextBlock*`

The text block widget used to display the question text.

---

### `UMultiLineEditableTextBox* AnswerInput`

**Type:** `UMultiLineEditableTextBox*`

The multi-line editable text box where the user types their answer.

---

### `UButton* SubmitButton`

**Type:** `UButton*`

The button widget to submit the answer.
