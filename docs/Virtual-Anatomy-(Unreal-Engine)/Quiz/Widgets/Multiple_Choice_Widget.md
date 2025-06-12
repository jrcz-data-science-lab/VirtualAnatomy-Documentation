# UCPP_MultipleChoiceWidget

* **File**: `VirtualAnatomy/Public/Widgets/Quiz/Questions/CPP_MultipleChoiceWidget.h` & `VirtualAnatomy/Private/Widgets/Quiz/Questions/CPP_MultipleChoiceWidget.cpp`
* **Purpose**: Displays multiple-choice questions to the user.

## Public Methods

### `void NativeConstruct()`

Called when the widget is constructed. It ensures that the `AnswerListBox` is properly set.

---

### `void InitializeWithQuestion(const FQuizQuestion& Question)`

**Parameters:**

-   `const FQuizQuestion& Question` - The quiz question to display in the widget.

Initializes the widget with the provided quiz question, setting up the UI elements and preparing the widget for displaying the question and answers. It also calls `GenerateAnswerButtons`.

---

### `int32 LastSelectedAnswerIndex`

**Type:** `int32`
**Default Value:** `-1`

The index of the last selected answer button.

## Protected Properties

### `UTextBlock* QuestionText`

**Type:** `UTextBlock*`

The text block widget used to display the question text.

---

### `UVerticalBox* AnswerListBox`

**Type:** `UVerticalBox*`

The vertical box widget that contains the answer buttons.

---

### `TSubclassOf<UCPP_AnswerButtonWidget> AnswerButtonClass`

**Type:** `TSubclassOf<UCPP_AnswerButtonWidget>`

The Blueprint class for individual answer button widgets.

## Protected Methods

### `void GenerateAnswerButtons()`

Creates the answer buttons based on the provided question data and adds them to the `AnswerListBox`.

---

### `void HandleAnswerClicked(const FQuizAnswer& SelectedAnswer)`

**Parameters:**

-   `const FQuizAnswer& SelectedAnswer` - The answer that was clicked by the user.

Handles the event when an answer button is clicked, stores the index of the selected answer, and broadcasts the `OnQuestionAnswered` delegate.
