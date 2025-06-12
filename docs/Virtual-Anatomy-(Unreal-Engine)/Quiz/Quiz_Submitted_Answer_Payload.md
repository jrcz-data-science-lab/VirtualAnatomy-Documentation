# FQuizSubmittedAnswerPayload

* **File**: `VirtualAnatomy/Public/Quiz/QuizSubmissionPayload.h`
* **Purpose**: Stores a single user's answer to a question in a format ready for API submission. Only one of the `SelectedAnswerId_Index`, `ResponseText_ClickedMesh_ID`, or `ResponseText_ShortAnswer` fields is typically populated per answer.

## Public Properties

### `FString Question_ID`

**Type:** `FString`

The MongoDB `_id` string of the question.

---

### `int32 SelectedAnswerId_Index`

**Type:** `int32`
**Default Value:** `-1`

For multiple-choice or true/false questions, this is the index of the selected answer. Defaults to -1 if not used.

---

### `FString ResponseText_ClickedMesh_ID`

**Type:** `FString`

For "select-organ" questions, this is the MongoDB `_id` string of the clicked mesh.

---

### `FString ResponseText_ShortAnswer`

**Type:** `FString`

For short-answer (open-ended) questions, this is the user's text response.

## Public Methods

### `FQuizSubmittedAnswerPayload()`

Constructor that initializes `SelectedAnswerId_Index` to `-1`.
