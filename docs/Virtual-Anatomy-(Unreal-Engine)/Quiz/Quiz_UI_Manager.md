# UQuizUIManager

* **File**: `VirtualAnatomy/Public/Quiz/QuizUIManager.h` & `VirtualAnatomy/Private/Quiz/QuizUIManager.cpp`
* **Purpose**: The central controller for the quiz UI. It handles the display and navigation between different quiz screens, manages the quiz state, and orchestrates answer submission.

## Public Methods

### `void Initialize(APlayerController* InPlayerController)`

**Parameters:**

-   `APlayerController* InPlayerController` - The player controller to associate with this UI manager.

Initializes the Quiz UI Manager with the given player controller, setting up UI elements and preparing the manager for use. It also loads the `UMeshKnowledgeBaseDataAsset` and presents the initial study year selection.

---

### `void ShowQuizOverview()`

Displays the quiz overview screen by creating and adding the quiz overview widget to the viewport.

---

### `void ShowQuizDetails(const FQuizData& Quiz)`

**Parameters:**

-   `const FQuizData& Quiz` - The quiz data to display in the details widget.

Displays the details of a specific quiz by creating and adding the quiz details widget to the viewport.

---

### `void StartQuiz(const FQuizData& Quiz)`

**Parameters:**

-   `const FQuizData& Quiz` - The quiz data to start.

Starts the quiz with the provided quiz data, initializing the quiz and displaying the first question.

---

### `void ShowNextQuestion()`

Displays the next question in the quiz by creating and adding the appropriate question widget to the viewport. If all questions are answered, it finalizes and submits the quiz.

---

### `void ShowQuizEndScreen()`

Displays the quiz end screen by creating and adding the quiz end screen widget to the viewport.

---

### `void HandleStudyYearSelected(const int32 StudyYear)`

**Parameters:**

-   `const int32 StudyYear` - The selected study year.

Handles the selection of a study year, updates the `SelectedStudyYear` property, and fetches quizzes for the chosen year.

---

### `void HandleQuestionAnswered(const FQuizQuestion& QuestionData, const FQuizSubmittedAnswerPayload& AnswerPayload)`

**Parameters:**

-   `const FQuizQuestion& QuestionData` - The `FQuizQuestion` data for the question that was just answered.
-   `const FQuizSubmittedAnswerPayload& AnswerPayload` - The `FQuizSubmittedAnswerPayload` containing the student's formatted answer for the question.

Processes a successfully answered question by adding the submitted answer to a list of accumulated answers, incrementing the question index, and then proceeding to display the next question or finalize the quiz.

---

### `UQuizUIManager()`

Constructor for the Quiz UI Manager, which initializes default values and loads Blueprint classes for various quiz widgets.

## Public Properties

### `int32 SelectedStudyYear`

**Type:** `int32`
**Default Value:** `0`

The study year currently selected by the user.

---

### `UQuizManager* QuizManagerInstance`

**Type:** `UQuizManager*`

A pointer to the instance of the `UQuizManager` used to fetch and manage quizzes.

---

### `TSoftObjectPtr<UMeshKnowledgeBaseDataAsset> MeshKnowledgeAssetPath`

**Type:** `TSoftObjectPtr<UMeshKnowledgeBaseDataAsset>`

Path to the Data Asset containing the mesh knowledge base, used for "select organ" question types.

---

### `UMeshKnowledgeBaseDataAsset* LoadedMeshKnowledgeAsset`

**Type:** `UMeshKnowledgeBaseDataAsset*`

A transient pointer to the loaded `UMeshKnowledgeBaseDataAsset`.

## Protected Properties

### `APlayerController* PlayerController`

**Type:** `APlayerController*`

The player controller associated with this UI manager, used for creating and adding widgets to the viewport.

---

### `TSubclassOf<UCPP_QuizOverview> QuizOverviewClass`

**Type:** `TSubclassOf<UCPP_QuizOverview>`

Blueprint class for the quiz overview widget.

---

### `TSubclassOf<UCPP_QuizDetailsWidget> QuizDetailsClass`

**Type:** `TSubclassOf<UCPP_QuizDetailsWidget>`

Blueprint class for the quiz details widget.

---

### `TSubclassOf<UCPP_QuizQuestionBase> MultipleChoiceWidgetClass`

**Type:** `TSubclassOf<UCPP_QuizQuestionBase>`

Blueprint class for the multiple-choice question widget.

---

### `TSubclassOf<UCPP_QuizQuestionBase> TrueFalseWidgetClass`

**Type:** `TSubclassOf<UCPP_QuizQuestionBase>`

Blueprint class for the true/false question widget.

---

### `TSubclassOf<UCPP_QuizQuestionBase> SelectOrganWidgetClass`

**Type:** `TSubclassOf<UCPP_QuizQuestionBase>`

Blueprint class for the "select organ" question widget.

---

### `TSubclassOf<UCPP_QuizQuestionBase> OpenEndedWidgetClass`

**Type:** `TSubclassOf<UCPP_QuizQuestionBase>`

Blueprint class for the open-ended question widget.

---

### `TSubclassOf<UUserWidget> QuizEndScreenClass`

**Type:** `TSubclassOf<UUserWidget>`

Blueprint class for the quiz end screen widget.

---

### `UCPP_QuizOverview* CurrentOverview`

**Type:** `UCPP_QuizOverview*`

A pointer to the currently active quiz overview widget.

---

### `UCPP_QuizDetailsWidget* CurrentDetails`

**Type:** `UCPP_QuizDetailsWidget*`

A pointer to the currently active quiz details widget.

---

### `UCPP_QuizQuestionBase* ActiveQuestionWidget`

**Type:** `UCPP_QuizQuestionBase*`

A pointer to the currently active quiz question widget.

---

### `int32 CurrentQuestionIndex`

**Type:** `int32`
**Default Value:** `0`

The index of the current question being displayed within the active quiz.

---

### `FQuizData CurrentQuiz`

**Type:** `FQuizData`

The data for the quiz that is currently active or being viewed.

---

### `TArray<FQuizSubmittedAnswerPayload> AccumulatedAnswers`

**Type:** `TArray<FQuizSubmittedAnswerPayload>`

A list of all answers submitted by the user for the current quiz.

## Private Methods

### `void HandleQuestionAnsweredInternalBinder()`

This function is connected to the `OnQuestionAnswered` delegate of the currently active question widget. It determines the type of the active question widget, extracts the student's answer in the appropriate format, and then calls the main `HandleQuestionAnswered` method.

---

### `void FinalizeAndSubmitQuiz()`

This function is called when all questions in the current quiz have been answered. It constructs the `FQuizSubmissionPayload` and then calls `SendQuizSubmissionToAPI` to transmit the data.

---

### `void SendQuizSubmissionToAPI(const FQuizSubmissionPayload& SubmissionPayload)`

**Parameters:**

-   `const FQuizSubmissionPayload& SubmissionPayload` - The struct containing all information for the quiz submission.

Sends the quiz submission data to the backend API by serializing it into a JSON string and creating an HTTP POST request.

---

### `void OnQuizSubmissionResponseReceived(FHttpRequestPtr Request, FHttpResponsePtr Response, bool bWasSuccessful)`

**Parameters:**

-   `FHttpRequestPtr Request` - The original HTTP request object.
-   `FHttpResponsePtr Response` - The HTTP response object from the server.
-   `bool bWasSuccessful` - Boolean indicating if the HTTP request itself was successful.

Handles the response received from the backend API after a quiz submission attempt. It logs success or failure, clears accumulated answers, and navigates to the quiz overview.

## Private Properties

### `TSubclassOf<UUserWidget> StudyYearSelectionWidgetClass`

**Type:** `TSubclassOf<UUserWidget>`

Blueprint class for the study year selection widget.

---

### `void ShowStudyYearSelectionWidget()`

Displays the study year selection widget, removing any other active quiz widgets.

---

### `void LoadMeshKnowledgeAsset()`

Loads the `UMeshKnowledgeBaseDataAsset` asynchronously or synchronously if it's already valid.
