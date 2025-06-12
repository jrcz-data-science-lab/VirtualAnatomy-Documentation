# UCPP_SelectOrganWidget

* **File**: `VirtualAnatomy/Public/Widgets/Quiz/Questions/CPP_SelectOrganWidget.h` & `VirtualAnatomy/Private/Widgets/Quiz/Questions/CPP_SelectOrganWidget.cpp`
* **Purpose**: Displays questions that require the user to click on a specific anatomical mesh in the 3D environment.

## Public Methods

### `void InitializeWithQuestion(const FQuizQuestion& Question)`

**Parameters:**

-   `const FQuizQuestion& Question` - The quiz question data to initialize the widget with.

Initializes the widget with the provided quiz question, setting the question text and subscribing to the `UMeshSelector::OnMeshClicked` delegate.

---

### `FString LastClickedMeshDB_ID`

**Type:** `FString`

Stores the database ID of the last clicked mesh for submission.

---

### `FString LastClickedMeshNameKey_ForCatalogLookup`

**Type:** `FString`

Stores the name key of the last clicked mesh, used for lookup in the Mesh Knowledge Base.

---

### `AActor* SelectedOrgan`

**Type:** `AActor*`

A pointer to the actor representing the last selected organ.

## Protected Methods

### `void NativeConstruct()`

Called when the widget is constructed. It binds the `OnSubmitButtonClicked` event and attempts to get a reference to `UQuizUIManager`.

---

### `UTextBlock* QuestionText`

**Type:** `UTextBlock*`

The text block widget used to display the question text.

---

### `UButton* SubmitButton`

**Type:** `UButton*`

The button widget to submit the selected organ.

---

### `void OnMeshClicked(AActor* ClickedOwningActor)`

**Parameters:**

-   `AActor* ClickedOwningActor` - The owning actor of the mesh that was clicked.

Handles the event when a mesh in the 3D environment is clicked. It updates `SelectedOrgan`, identifies the clicked mesh's name, and retrieves its database ID using the `UMeshKnowledgeBaseDataAsset`.

---

### `void OnSubmitButtonClicked()`

Handles the click event for the Submit button. It performs validation on the selected organ against the expected target and then broadcasts the `OnQuestionAnswered` delegate.

## Private Properties

### `FString CurrentQuestionTargetType`

**Type:** `FString`

Stores the expected target type for the current question ("mesh" or "group").

---

### `FString CurrentQuestionTargetID`

**Type:** `FString`

Stores the MongoDB `_id` of the expected target for the current question.

---

### `UQuizUIManager* QuizUIManagerRef`

**Type:** `UQuizUIManager*`

A reference to the `UQuizUIManager` for accessing the Mesh Knowledge Base.
