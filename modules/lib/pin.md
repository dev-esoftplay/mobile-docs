# LibPin

This component provides a PIN input field with customizable styling. It allows users to enter a PIN of a specified length and provides a callback function to handle PIN changes.

## Installation

This component does not have any external dependencies beyond React Native itself.

## Usage

```javascript
import { LibPin } from 'esoftplay/cache/lib/pin/import';

<LibPin
  length={4}
  onChangePin={(pin) => console.log("inserted changed:", pin)}
  boxStyle={{ backgroundColor: '#eee' }} // Optional: Style for the PIN boxes
  pinStyle={{ backgroundColor: 'blue' }} // Optional: Style for the PIN dots
  overrideKeyboard={true} // Optional: Override the default keyboard input
  pinValue={'1234'} // Optional: Set initial pin value
/>
```

## Props

| Prop             | Type        | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| ---------------- | ----------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `length`         | `number`    | Yes      | The length of the PIN.                                                                                                                                                                                                                                                                                                                                                                                     |
| `onChangePin`   | `(pin: string) => void` | Yes      | Callback function called when the PIN changes. Receives the current PIN as a string.                                                                                                                                                                                                                                                                                                                                                              |
| `boxStyle`       | `ViewStyle` | No       | Optional style for the boxes that display the PIN digits.                                                                                                                                                                                                                                                                                                                                                             |
| `overrideKeyboard` | `boolean`   | No       | Optional. If `true`, the default numeric keyboard input is overridden. This can be used if you want to implement a custom PIN input method (e.g., using buttons).                                                                                                                                                                                                                                                                                                                                                             |
| `pinValue`       | `string`    | No       | Optional initial value for the pin. Useful for pre-filling the pin or setting it programmatically.                                                                                                                                                                                                                                                                                                                                                                |
| `pinStyle`       | `ViewStyle` | No       | Optional style for the dots that represent the entered digits within each box.                                                                                                                                                                                                                                                                                                                                                             |

## Functionality

1. **PIN Display:** The component renders a series of empty boxes, one for each digit of the PIN.  As the user enters digits, filled circles appear in the boxes.

2. **Hidden `TextInput`:** A hidden `TextInput` is used to capture the user's input.  It is styled to be transparent and positioned off-screen.  This is the standard way to handle PIN input in React Native, as it allows access to the device's secure input methods.

3. **`onChangePin` Callback:** The `onChangePin` callback is called whenever the PIN changes.  It receives the current PIN as a string.

4. **Keyboard Input:** By default, the component uses the device's numeric keyboard.

5. **Custom Input (Override Keyboard):** If the `overrideKeyboard` prop is set to `true`, the hidden `TextInput` is not rendered. This allows you to implement a custom PIN input method using buttons or other UI elements.  You would then need to manually update the `pin` state and call the `onChangePin` callback.

6. **Initial Value:** The `pinValue` prop can be used to set the initial value of the PIN.  This is useful for pre-filling the PIN or setting it programmatically.  The component uses a `useEffect` hook to update the internal `pin` state and call the `onChangePin` callback whenever the `pinValue` prop changes.

## Important Considerations

*   **Security:**  The `TextInput` uses `secureTextEntry` to mask the PIN digits.  However, it's crucial to follow security best practices when handling PINs.  Avoid storing PINs directly in your app's code or local storage.  Use secure methods for transmitting and storing PINs (e.g., hashing and salting).
*   **Custom Input:**  If you use `overrideKeyboard={true}`, you are responsible for implementing the entire PIN input logic, including handling user input, updating the display, and calling the `onChangePin` callback.
*   **Focus:** The component focuses the `TextInput` on mount using a `setTimeout` to ensure the component is rendered before attempting to focus.
*   **Accessibility:** Consider accessibility.  Use appropriate ARIA attributes to label the PIN input field and provide feedback to users with visual impairments.  For custom input methods, ensure that the controls are accessible via keyboard.
*   **Styling:** The `boxStyle` and `pinStyle` props allow you to customize the appearance of the PIN input.
*   **Controlled Component:** The PIN value is managed internally by the component's state.  The `pinValue` prop allows you to set the initial value or update it from a parent component, making it a controlled component.  The `useEffect` hook ensures that the internal state is updated when the `pinValue` prop changes.
