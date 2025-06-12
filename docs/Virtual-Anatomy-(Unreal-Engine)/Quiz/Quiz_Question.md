# FQuizQuestion

* **File**: `VirtualAnatomy/Public/Quiz/QuizQuestion.h` & `VirtualAnatomy/Private/Quiz/QuizQuestion.cpp`
* **Purpose**: Represents a single question within a quiz.

## Public Properties

### `FString Id`

**Type:** `FString`

The unique identifier for the question (MongoDB `_id`).

---

### `FString Question`

**Type:** `FString`

The text of the question.

---

### `FString Type`

**Type:** `FString`

The type of the question (e.g., "multiple-choice", "true-false", "select-organ", "short-answer").

---

### `TArray<FQuizAnswer> Answers`

**Type:** `TArray<FQuizAnswer>`

An array of possible answers for this question (relevant for multiple-choice and true/false).

---

### `FString CurrentExpectedTargetType`

**Type:** `FString`

For "select-organ" questions, specifies whether the expected target is a "mesh" or a "group".

---

### `FString CurrentExpectedTargetID`

**Type:** `FString`

For "select-organ" questions, the MongoDB `_id` of the expected target (e.g., an organ or group).
