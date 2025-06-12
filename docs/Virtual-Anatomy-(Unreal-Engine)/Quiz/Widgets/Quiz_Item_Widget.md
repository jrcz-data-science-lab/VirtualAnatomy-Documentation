# UCPP_QuizItemWidget

* **File**: `VirtualAnatomy/Public/Widgets/Quiz/CPP_QuizItemWidget.h` & `VirtualAnatomy/Private/Widgets/Quiz/CPP_QuizItemWidget.cpp`
* **Purpose**: A user widget class that represents a single quiz item in the UI. It displays the quiz title and date and provides functionality for handling user interaction.

## Public Methods

### `void InitializeWithData(const FQuizData& Quiz)`

**Parameters:**

-   `const FQuizData& Quiz` - The quiz data to populate the widget with.

Initializes the widget with the provided quiz data, setting the title and formatted date.

---

### `void NativeConstruct()`

Called when the widget is constructed. It sets up bindings and initializes the widget's state.

---

### `void HandleClick()`

Handles the click event for the associated button, broadcasting the `OnQuizItemClicked` delegate.

---

### `FOnQuizItemClicked OnQuizItemClicked`

**Type:** `FOnQuizItemClicked` (Delegate)

Delegate broadcast when the quiz item button is clicked, passing the `FQuizData` of the clicked item.

## Protected Properties

### `UTextBlock* TitleText`

**Type:** `UTextBlock*`

The text block widget used to display the quiz title.

---

### `UTextBlock* DateText`

**Type:** `UTextBlock*`

The text block widget used to display the scheduled date of the quiz.

---

### `UButton* ClickButton`

**Type:** `UButton*`

The button widget that triggers the click event for the quiz item.

---

### `FQuizData StoredQuizData`

**Type:** `FQuizData`

The quiz data stored by this widget instance.
