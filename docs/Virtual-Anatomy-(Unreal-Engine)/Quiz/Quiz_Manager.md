# UQuizManager

* **File**: `VirtualAnatomy/Public/Quiz/QuizManager.h` & `VirtualAnatomy/Private/Quiz/QuizManager.cpp`
* **Purpose**: Manages asynchronous HTTP requests to fetch quizzes and handles the parsing of JSON responses.
* **Key Functions**:
    * `FetchQuizzes(int32 StudyYear)`: Initiates a GET request to the quiz API endpoint, optionally filtered by `StudyYear`.
    * `OnResponseReceived(FHttpRequestPtr Request, FHttpResponsePtr Response, bool bWasSuccessful)`: Callback function for HTTP request completion. It deserializes the JSON response into an array of `FQuizData` structs.
* **Delegates**:
    * `FOnQuizzesFetched OnQuizzesFetched`: Broadcasts a `TArray<FQuizData>` when quizzes are successfully fetched and parsed.
