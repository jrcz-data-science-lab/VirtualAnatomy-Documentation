# UCPP_QuizEndScreenWidget

* **File**: `VirtualAnatomy/Public/Widgets/Quiz/CPP_QuizEndScreenWidget.h` & `VirtualAnatomy/Private/Widgets/Quiz/CPP_QuizEndScreenWidget.cpp`
* **Purpose**: Displays a message to the user upon quiz completion and provides a button to navigate back to the main menu.

## Public Methods

### `void NativeConstruct()`

Called when the widget is constructed. It binds the `OnMainMenuButtonClicked` event and sets the default end-of-quiz message.

## Protected Properties

---

### `UTextBlock* EndOfQuizText`

**Type:** `UTextBlock*`

Text block to display the end of quiz message.

---

### `UButton* MainMenuButton`

**Type:** `UButton*`

Button to navigate back to the main menu.

## Protected Methods

### `void OnMainMenuButtonClicked()`

Handles the click event for the main menu button, which loads the "MainMenuLevel".
