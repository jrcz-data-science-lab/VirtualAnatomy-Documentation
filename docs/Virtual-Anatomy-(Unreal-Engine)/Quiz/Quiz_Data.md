# FQuizData

* **File**: `VirtualAnatomy/Public/Quiz/QuizData.h` & `VirtualAnatomy/Private/Quiz/QuizData.cpp`
* **Purpose**: Represents a complete quiz.

## Public Properties

### `FString Id`

**Type:** `FString`

The unique identifier of the quiz (MongoDB `_id`).

---

### `FString Title`

**Type:** `FString`

The title of the quiz.

---

### `int32 QuizDesignatedStudyYear`

**Type:** `int32`

The designated study year for the quiz (e.g., 1st, 2nd, etc.).

---

### `TArray<FQuizQuestion> Questions`

**Type:** `TArray<FQuizQuestion>`

An array containing all the questions in the quiz.

---

### `FString ScheduledAt`

**Type:** `FString`

The scheduled date and time for the quiz, in ISO 8601 format.
