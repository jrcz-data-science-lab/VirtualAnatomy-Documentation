# UCPP_QuizDetailsWidget

* **File**: `VirtualAnatomy/Public/Widgets/Quiz/CPP_QuizDetailsWidget.h` & `VirtualAnatomy/Private/Widgets/Quiz/CPP_QuizDetailsWidget.cpp`
* **Purpose**: A user widget class that displays detailed information about a quiz. It includes the quiz title, scheduled date, and a list of questions. The widget also provides a button to start the quiz.

## Public Methods

### `void NativeConstruct()`

Called when the widget is constructed.

---

### `void InitializeWithData(const FQuizData& Quiz)`

**Parameters:**

-   `const FQuizData& Quiz` - The quiz data to populate the widget with.

Initializes the widget's state and displays the quiz information, including title, scheduled date, and question list.

---

### `void SetOnStartQuizCallback(TFunction<void()> Callback)`

**Parameters:**

-   `TFunction<void()> Callback` - The callback function to be called on quiz start.

Sets the callback function to be executed when the quiz is started.

---

### `FQuizData GetCurrentQuiz() const`

**Returns:** `FQuizData` - The quiz data currently displayed in the widget.

Gets the current quiz data stored by this widget.

## Protected Properties

### `UTextBlock* TitleText`

**Type:** `UTextBlock*`

The text block widget for displaying the quiz title.

---

### `UTextBlock* ScheduledText`

**Type:** `UTextBlock*`

The text block widget for displaying the quiz's scheduled date.

---

### `UTextBlock* QuestionText`

**Type:** `UTextBlock*`

The text block widget for displaying the list of questions in the quiz.

---

### `UButton* StartQuizButton`

**Type:** `UButton*`

The button widget that initiates the quiz start process.

---

### `TFunction<void()> OnStartQuiz`

**Type:** `TFunction<void()>`

A callback function that is invoked when the "Start Quiz" button is clicked.

---

### `FQuizData CurrentQuiz`

**Type:** `FQuizData`

The quiz data currently displayed by this widget.

## Protected Methods

### `void OnStartQuizButtonClicked()`

Handles the click event for the Start Quiz button, which removes the widget from parent and triggers the `OnStartQuiz` callback.
