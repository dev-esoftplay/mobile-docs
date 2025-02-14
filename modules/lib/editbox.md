# LibEditbox

This component provides a modal input box for React Native. It allows users to input text and submit it with a customizable label and keyboard type.  It uses a global state to control visibility and properties.

## Installation

This component depends on `esoftplay`. Ensure you have it installed in your project.

```bash
npm install esoftplay
# or
yarn add esoftplay
```

## Usage

```javascript
import { LibEditbox } from 'esoftplay/cache/lib/editbox/import';

// To show the editbox:
LibEditbox.show(
  "Enter your text", // Label
  "Initial text",  // Default value
  "default",        // Keyboard type ("default", "email-address", "numeric", "phone-pad")
  (text) => {       // Submit callback
    console.log("Submitted text:", text);
    // Do something with the submitted text
  }
);

// To hide the editbox:
LibEditbox.hide();
```

## API

### Static Methods

#### **`LibEditbox.show(label, defaultValue, keyboardType, onSubmit)`**
Shows the editbox modal.
* `label` (string): The label for the input field.
* `defaultValue` (string): The initial text in the input field.
* `keyboardType` (string): The keyboard type for the input.
* `onSubmit` (function): Callback function called when the user submits the text. Receives the entered text as an argument.

#### **`LibEditbox.hide()`**
 Hides the editbox modal.

### Props

The `LibEditbox` component itself doesn't directly receive props. The editbox's properties are managed through the static `show` method and a global state.  The `LibEditbox` component renders the inner component, which uses the global state.

## `ComponentEditboxProps` Interface

This interface defines the props that are *internally* used by the input component within the modal:

```typescript
export interface ComponentEditboxProps {
  defaultValue?: string,
  onSubmit?: (e: string) => void,
  label?: string,
  placeholder?: string,
  error?: string,
  helper?: string
  allowFontScaling?: boolean,
  autoCapitalize?: "none" | "sentences" | "words" | "characters",
  autoCorrect?: boolean,
  autoFocus?: boolean,
  blurOnSubmit?: boolean,
  caretHidden?: boolean,
  contextMenuHidden?: boolean,
  editable?: boolean,
  inactive?: boolean,
  keyboardType?: "default" | "email-address" | "numeric" | "phone-pad",
  maxLength?: number,
  multiline?: boolean,
  onSubmitEditing?: () => void,
  onChangeText?: (text: string) => void,
  placeholderTextColor?: string,
  returnKeyType?: "done" | "go" | "next" | "search" | "send",
  secureTextEntry?: boolean,
  selectTextOnFocus?: boolean,
  selectionColor?: string,
  style?: StyleProp<TextStyle>,
  value?: string,
}
```

These props are passed to the `LibInput` component.

## Global State

The editbox's visibility and properties are managed using a global state (from `esoftplay`). This allows the editbox to be controlled from anywhere in the application using the static methods.

## Functionality

1. **Modal Presentation:** The editbox is presented as a modal overlay.

2. **Input Component:** The `LibInput` component (from `esoftplay`) is used for the text input.

3. **Submit Handling:** The `onSubmit` callback is called when the user submits the text (by pressing the button).

4. **Keyboard Avoidance:** The `LibKeyboard_avoid` component (from `esoftplay`) is used to prevent the keyboard from obscuring the input field.

5. **Styling:** The styling is handled using `LibTheme`, `LibStyle`, etc. (from `esoftplay`).

6. **Global State Management:** The global state is used to store and manage the editbox's properties (visibility, label, default value, etc.).

## Important Considerations

* **`esoftplay` Dependency:** This component is tightly coupled to the `esoftplay` library. Ensure it is correctly set up in your project. All modules should be imported directly from `esoftplay`.
* **Global State:** The use of a global state can simplify the component's implementation, but it might have implications for state management in larger applications.
* **Modal Overlay:** The editbox is presented as a modal overlay, which might affect the user experience depending on the context.
* **Keyboard Avoidance:** The `LibKeyboard_avoid` component helps to ensure the input field is visible when the keyboard is shown.
* **Styling:** The styling relies on `esoftplay`'s styling modules.
* **Input Props:** The `ComponentEditboxProps` interface defines the props that are passed to the underlying `LibInput` component.  You can customize the input's behavior and appearance using these props.
* **Ref:**  The `input` ref is used to access the `LibInput` component's methods (e.g., `setText`, `focus`, `getText`).
* **Accessibility:** Consider adding accessibility features to the editbox, such as labels, ARIA attributes, and keyboard navigation support.
