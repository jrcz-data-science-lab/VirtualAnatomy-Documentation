# UCPP_AnswerButtonWidget

* **File**: `VirtualAnatomy/Public/Widgets/Quiz/Questions/CPP_AnswerButtonWidget.h` & `VirtualAnatomy/Private/Widgets/Quiz/Questions/CPP_AnswerButtonWidget.cpp`
* **Purpose**: Represents a single answer button within a multiple-choice question.

## Public Methods

### `void NativeConstruct()`

Called when the widget is constructed. It binds the `OnClicked` event to the `AnswerButton`.

---

### `void SetAnswerData(const FQuizAnswer& InAnswer)`

**Parameters:**

-   `const FQuizAnswer& InAnswer` - The `FQuizAnswer` data to populate the button with.

Sets the answer data for the widget, updating the displayed text.

---

### `FOnAnswerSelected OnAnswerSelected`

**Type:** `FOnAnswerSelected` (Delegate)

Delegate broadcast when this answer button is clicked, passing its `FQuizAnswer` data.

## Protected Properties

### `class UButton* AnswerButton`

**Type:** `class UButton*`

The button widget component.

---

### `class UTextBlock* AnswerText`

**Type:** `class UTextBlock*`

The text block widget displaying the answer text.

## Protected Methods

### `void OnClicked()`

Handles the click event of the `AnswerButton`, broadcasting the `OnAnswerSelected` delegate.

---

### `FQuizAnswer StoredAnswer`

**Type:** `FQuizAnswer`

The `FQuizAnswer` data stored by this widget instance.
