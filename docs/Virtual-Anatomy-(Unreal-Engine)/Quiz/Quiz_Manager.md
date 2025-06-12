# UQuizManager

* **File**: `VirtualAnatomy/Public/Quiz/QuizManager.h` & `VirtualAnatomy/Private/Quiz/QuizManager.cpp`
* **Purpose**: Manages asynchronous HTTP requests to fetch quizzes and handles the parsing of JSON responses.

## Public Methods

### `void FetchQuizzes(int32 StudyYear)`

**Parameters:**

-   `int32 StudyYear` - The academic study year for which quizzes should be fetched.

Initiates a GET request to the quiz API endpoint, optionally filtered by `StudyYear`.

## Public Properties

### `FOnQuizzesFetched OnQuizzesFetched`

**Type:** `FOnQuizzesFetched` (Delegate)

Broadcasts a `TArray<FQuizData>` when quizzes are successfully fetched and parsed.

---

### `UQuizManager* QuizManager`

**Type:** `UQuizManager*`

A self-referential pointer to a `UQuizManager` instance. Its specific use case beyond referencing itself within the class might be for a manager of managers, but in this context, it appears as a direct member.

## Private Methods

### `void OnResponseReceived(FHttpRequestPtr Request, FHttpResponsePtr Response, bool bWasSuccessful)`

**Parameters:**

-   `FHttpRequestPtr Request` - A pointer to the HTTP request that was completed.
-   `FHttpResponsePtr Response` - A pointer to the HTTP response received from the server.
-   `bool bWasSuccessful` - A boolean indicating if the HTTP request was successful.

This is a callback function for HTTP request completion. It deserializes the JSON response into an array of `FQuizData` structs and broadcasts them.
