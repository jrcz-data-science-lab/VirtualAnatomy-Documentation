# UCPP_QuizOverview

* **File**: `VirtualAnatomy/Public/Widgets/Quiz/CPP_QuizOverview.h` & `VirtualAnatomy/Private/Widgets/Quiz/CPP_QuizOverview.cpp`
* **Purpose**: A user widget class responsible for displaying an overview of quizzes. It manages the UI elements for listing quizzes and handles the logic for fetching and displaying quiz data.

## Public Methods

### `void NativeConstruct()`

Called when the widget is constructed.

---

### `void SetOnQuizSelectedCallback(TFunction<void(const FQuizData&)> Callback)`

**Parameters:**

-   `TFunction<void(const FQuizData&)> Callback` - The callback function to be executed when a quiz is selected.

Sets the callback function to be called when a quiz item is selected in the overview.

---

### `void HandleQuizzesFetched(const TArray<FQuizData>& FetchedQuizzes)`

**Parameters:**

-   `const TArray<FQuizData>& FetchedQuizzes` - The array of quiz data fetched from the server.

Handles the event when quizzes are fetched successfully. It populates the quiz list UI with the fetched quiz data.

## Protected Properties

### `UVerticalBox* QuizListBox`

**Type:** `UVerticalBox*`

The vertical box widget that contains the list of quiz items.

---

### `TSubclassOf<UUserWidget> QuizListBoxClass`

**Type:** `TSubclassOf<UUserWidget>`

The Blueprint class for individual quiz list items.

---

### `UQuizManager* QuizManager`

**Type:** `UQuizManager*`

A pointer to the `UQuizManager` instance.

---

### `TArray<FQuizData> Quizzes`

**Type:** `TArray<FQuizData>`

An array storing the `FQuizData` for all fetched quizzes.

---

### `TFunction<void(const FQuizData&)> OnQuizSelected`

**Type:** `TFunction<void(const FQuizData&)>`

A callback function that is invoked when a quiz item is selected, passing the selected quiz data.

---

### `void HandleQuizItemClicked(const FQuizData& QuizData)`

**Parameters:**

-   `const FQuizData& QuizData` - The quiz data associated with the clicked quiz item.

Handles the event when a quiz item is clicked. This function is bound to the `OnQuizItemClicked` event of the quiz item widget.
