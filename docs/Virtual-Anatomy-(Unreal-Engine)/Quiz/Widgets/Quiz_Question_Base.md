# UCPP_QuizQuestionBase (Base Class)

* **File**: `VirtualAnatomy/Public/Widgets/Quiz/CPP_QuizQuestionBase.h` & `VirtualAnatomy/Private/Widgets/Quiz/CPP_QuizQuestionBase.cpp`
* **Purpose**: An abstract base class for all question-specific UI widgets within the quiz system.

## Public Methods

### `void InitializeWithQuestion(const FQuizQuestion& Question)`

**Parameters:**

-   `const FQuizQuestion& Question` - The quiz question data to initialize the widget with.

Initializes the widget with the provided quiz question data. This function should be overridden by child classes to set up their specific UI elements.

---

### `FQuizQuestion CurrentQuestion`

**Type:** `FQuizQuestion`

The `FQuizQuestion` data that this widget is currently displaying.

---

### `FOnQuestionAnswered OnQuestionAnswered`

**Type:** `FOnQuestionAnswered` (Delegate)

Delegate broadcast when the user has completed answering the question (e.g., clicked an answer, submitted text).
