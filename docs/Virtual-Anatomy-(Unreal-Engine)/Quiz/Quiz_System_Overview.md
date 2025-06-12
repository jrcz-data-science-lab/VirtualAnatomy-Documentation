# Quiz System Documentation

The Virtual Anatomy project includes a comprehensive Quiz system designed to test user knowledge of human anatomy. This documentation outlines its architecture, data flow, and key components.

## 1. Architecture Overview

The Quiz system is built with a clear separation of concerns, utilizing several C++ classes to manage quiz data, UI flow, and interaction with a backend API.

Key architectural components include:

* **`UQuizManager`**: Handles all network communication with the backend API to fetch quizzes and submit results. It acts as the data layer for quizzes.
* **`UQuizUIManager`**: Manages the entire user interface flow for the quiz system, from year selection to displaying questions and the end screen. It orchestrates the display of various quiz widgets.
* **Quiz Data Structures**: C++ `USTRUCT`s define the format of quiz content and user submissions.
* **Quiz Widgets**: A hierarchy of `UUserWidget` classes handles the visual presentation and user interaction for different stages and types of quiz questions.
* **`UMeshKnowledgeBaseDataAsset`**: A data asset that provides a local lookup for anatomical mesh and group IDs, used by "select organ" question types.

## 2. Quiz System Data Flow

The typical flow of a user interacting with the Quiz system is as follows:

1.  **Initialization**: When the quiz level loads, `UQuizUIManager::Initialize` is called from the `ACPP_GameMode`. This loads the `UMeshKnowledgeBaseDataAsset` and presents the `UStudyYearSelectorWidget` to the user.
2.  **Study Year Selection**: The user selects a study year via the `UStudyYearSelectorWidget`. This selection triggers `UQuizUIManager::HandleStudyYearSelected`.
3.  **Fetching Quizzes**: `UQuizUIManager` instructs the `UQuizManager` to `FetchQuizzes` for the selected study year. The `UQuizManager` sends an HTTP GET request to the backend.
4.  **Quiz Overview Display**: Upon receiving a successful response, `UQuizManager` broadcasts the fetched quiz data. `UQuizUIManager`'s `CurrentOverview` widget (an `UCPP_QuizOverview`) listens to this event and populates a list of available quizzes.
5.  **Quiz Details**: The user selects a quiz from the overview. `UQuizUIManager::ShowQuizDetails` is called, displaying an `UCPP_QuizDetailsWidget` with more information about the chosen quiz.
6.  **Starting Quiz**: The user clicks "Start Quiz" on the details screen. `UQuizUIManager::StartQuiz` is called, initializing the quiz state (current question index, empty accumulated answers).
7.  **Displaying Questions**: `UQuizUIManager::ShowNextQuestion` determines the type of the current question (`FQuizQuestion::Type`) and dynamically creates the appropriate widget (`UCPP_MultipleChoiceWidget`, `UCPP_TrueFalseWidget`, `UCPP_OpenEndedWidget`, or `UCPP_SelectOrganWidget`). The question text and options are then initialized.
8.  **Answering Questions**: The user interacts with the active question widget (e.g., clicking an answer button, typing text, selecting an organ).
    * For "Select Organ" questions, `UCPP_SelectOrganWidget` subscribes to `UMeshSelector::OnMeshClicked` to capture user clicks on 3D anatomical models. It then uses the `UMeshKnowledgeBaseDataAsset` to validate or retrieve information about the clicked mesh.
9.  **Processing Answers**: When an answer is submitted on a question widget, its `OnQuestionAnswered` delegate is broadcast. `UQuizUIManager::HandleQuestionAnsweredInternalBinder` captures this, extracts the answer data in the correct format (`FQuizSubmittedAnswerPayload`), and then calls `UQuizUIManager::HandleQuestionAnswered`.
10. **Accumulating Answers**: `UQuizUIManager::HandleQuestionAnswered` adds the `FQuizSubmittedAnswerPayload` to an internal array of `AccumulatedAnswers` and increments the `CurrentQuestionIndex`. It then calls `ShowNextQuestion` again.
11. **Quiz Completion & Submission**: When all questions have been answered (`CurrentQuestionIndex` exceeds total questions), `UQuizUIManager::FinalizeAndSubmitQuiz` is called. This constructs the `FQuizSubmissionPayload` with all accumulated answers and a timestamp.
12. **Sending Submission**: `UQuizUIManager::SendQuizSubmissionToAPI` serializes the payload to JSON and sends an HTTP POST request to the backend `/api/submissions` endpoint.
13. **Submission Response**: `UQuizUIManager::OnQuizSubmissionResponseReceived` handles the backend's response, logging success or failure. Regardless of the outcome, it displays the `UCPP_QuizEndScreenWidget`.

## 3. Key Components

This section details the individual C++ classes that form the Quiz System.

### [UQuizManager](Quiz_Manager.md)
### [UQuizUIManager](Quiz_UI_Manager.md)
### [UMeshKnowledgeBaseDataAsset](Mesh_Knowledge_Base_Data_Asset.md)

## 4. Quiz Data Structures

This section details the C++ `USTRUCT`s that define the data models used throughout the quiz system.

### [FQuizData](Quiz_Data.md)
### [FQuizQuestion](Quiz_Question.md)
### [FQuizAnswer](Quiz_Answer.md)
### [FQuizSubmissionPayload](Quiz_Submission_Payload.md)
### [FMeshCatalogEntryUE and FOrganGroupUE](Mesh_And_Group_Data.md)

## 5. Quiz Widgets

This section details the UI components used in the Quiz System.

### [UStudyYearSelectorWidget](Study_Year_Selector_Widget.md)
### [UCPP_QuizOverview](Quiz_Overview.md)
### [UCPP_QuizItemWidget](Quiz_Item_Widget.md)
### [UCPP_QuizDetailsWidget](Quiz_Details_Widget.md)
### [UCPP_QuizQuestionBase](Quiz_Question_Base.md)
### [UCPP_MultipleChoiceWidget](Multiple_Choice_Widget.md)
### [UCPP_AnswerButtonWidget](Answer_Button_Widget.md)
### [UCPP_TrueFalseWidget](True_False_Widget.md)
### [UCPP_OpenEndedWidget](Open_Ended_Widget.md)
### [UCPP_SelectOrganWidget](Select_Organ_Widget.md)
### [UCPP_QuizEndScreenWidget](Quiz_End_Screen_Widget.md)
