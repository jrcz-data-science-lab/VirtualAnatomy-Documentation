# FQuizAnswer

* **File**: `VirtualAnatomy/Public/Quiz/QuizAnswer.h` & `VirtualAnatomy/Private/Quiz/QuizAnswer.cpp`
* **Purpose**: Represents a possible answer for a multiple-choice or true/false question.

## Public Properties

### `FString Id`

**Type:** `FString`

Unique identifier for the answer, can be used to match with submitted answers.

---

### `FString Text`

**Type:** `FString`

The text content of the answer.

---

### `bool bIsCorrect`

**Type:** `bool`

Indicates whether this answer is the correct one.
