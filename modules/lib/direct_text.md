# LibDirect_text

This component creates a read-only text input that can be directly manipulated using the `setText` method. It's useful for displaying text that needs to be updated programmatically without user interaction.

## Installation

This component relies on `esoftplay`. Ensure you have it installed in your project.

```bash
npm install esoftplay
# or
yarn add esoftplay
```

## Usage

```javascript
import { LibDirect_text } from 'esoftplay/cache/lib/direct_text/import';

// In your component:
const myTextRef = useRef<LibDirect_text>(null);

// ...

<LibDirect_text
  ref={myTextRef}
  style={{ fontSize: 16, color: 'black' }}
  initialText="Initial Text"
/>

// ...

// To update the text programmatically:
if (myTextRef.current) {
  myTextRef.current.setText("New Text");
}
```

## Props

| Prop          | Type                 | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| ------------- | -------------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `style`       | `StyleProp<TextStyle>` | No       | Style for the `TextInput` component.                                                                                                                                                                                                                                                                                                                                                                                    |
| `initialText` | `string`             | No       | The initial text to display in the input.                                                                                                                                                                                                                                                                                                                                                                           |

## Methods

### `setText(text: string): void`

Updates the text displayed in the input. This method is the primary way to change the text programmatically.

* **Parameters:**
    * `text`: The new text to display.

* **Returns:** `void`

## Functionality

1. **Read-Only Input:** The `TextInput` component is configured with `editable={false}`, making it read-only to the user.

2. **Direct Manipulation:** The `setText` method allows direct manipulation of the text displayed in the input.  It updates the `text` property of the component and uses `setNativeProps` to efficiently update the underlying native component.

3. **Ref:** A ref (`this.ref`) is used to access the `TextInput` component instance. This is necessary to call `setNativeProps`.

4. **Initial Text:** The `initialText` prop sets the initial text content.

5. **Styling:** The `style` prop allows customization of the input's appearance.

## Important Considerations

* **`esoftplay` Dependency:** This component extends `LibComponent` from `esoftplay`. Make sure `esoftplay` is correctly set up in your project.
* **Read-Only Nature:** The input is explicitly set to read-only.  Users cannot directly edit the text.
* **`setNativeProps`:**  The `setNativeProps` method is used for efficient updates.  It's generally more performant than calling `setState` in this scenario.
* **Ref Usage:** The ref is essential for accessing the `TextInput` instance and calling `setNativeProps`.
* **Uncontrolled Component:** This component is essentially an uncontrolled component. The text is stored in the component's `text` property, not in the state.  This simplifies the implementation but might have implications for complex scenarios. If you need more control over the text and want to use state management, you can modify it to be a controlled component.
* **Type Safety:** The use of `StyleProp<TextStyle>` and `string` for prop types enhances type safety.
* **Accessibility:** Consider adding accessibility features if needed, such as appropriate labels and ARIA attributes.  Since the input is read-only, you might need to provide alternative ways for users to access the content if they cannot interact with the text directly.
