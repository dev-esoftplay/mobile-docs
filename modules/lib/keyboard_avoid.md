# LibKeyboard_avoid

This component provides a cross-platform way to handle keyboard avoidance in React Native. It uses `KeyboardAvoidingView` and adjusts the behavior based on the platform (Android or iOS).

## Installation

This component relies on `react-native-keyboard-controller`. Ensure you have it installed:

```bash
npm install react-native-keyboard-controller
# or
yarn add react-native-keyboard-controller
```

## Usage

```javascript
import { LibKeyboard_avoid } from 'esoftplay/cache/lib/keyboard_avoid/import';

<LibKeyboard_avoid style={{ flex: 1, backgroundColor: 'white' }}>
  {/* Your content that needs to avoid the keyboard */}
  <TextInput placeholder="Enter text here" />
</LibKeyboard_avoid>
```

## Props

| Prop      | Type        | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| ----------- | ----------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `children` | `any`       | No       | The content to be rendered within the `KeyboardAvoidingView`. This is the part of your UI that you want to avoid being obscured by the keyboard.                                                                                                                                                                                                                                                                                                                                                               |
| `style`     | `ViewStyle` | No       | Additional styles for the `KeyboardAvoidingView`.  It's very common to want the content to fill the screen, so you'll often want to pass `style={{flex: 1}}` here.                                                                                                                                                                                                                                                                                                                                                      |

## Functionality

1. **Platform-Specific Behavior:** The component uses `Platform.OS` to determine the correct `behavior` prop for the `KeyboardAvoidingView`.
    * On Android, it uses `height`. This adjusts the height of the view to avoid the keyboard.
    * On iOS, it uses `padding`. This adds padding to the bottom of the view to push the content up above the keyboard.

2. **`KeyboardAvoidingView`:** The component renders a `KeyboardAvoidingView` with the appropriate `behavior` prop set based on the platform.

3. **Children Rendering:** The `children` prop is rendered inside the `KeyboardAvoidingView`.

## Important Considerations

* **`react-native-keyboard-controller` Dependency:** This component depends on the `react-native-keyboard-controller` library. Make sure it is correctly installed.
* **Platform Differences:** The `behavior` prop of `KeyboardAvoidingView` works differently on Android and iOS.  This component handles those differences for you.
* **Styling:**  The `style` prop is important for controlling the size and positioning of the view that avoids the keyboard.  You'll often want to use `flex: 1` in the style to make the view occupy the available space.
* **Content:**  The `children` prop should contain the content that you want to be visible when the keyboard appears.  This is typically your input fields and any related UI elements.
* **Nested `KeyboardAvoidingView`s:** Be careful about nesting `KeyboardAvoidingView` components.  This can sometimes lead to unexpected behavior.  In most cases, a single `KeyboardAvoidingView` at the top level of your screen is sufficient.
* **Alternatives:**  There are other libraries and approaches for handling keyboard avoidance in React Native.  However, this component provides a simple and cross-platform solution for most common use cases.
* **Accessibility:**  When the keyboard appears and covers part of the screen, ensure that users can still access and interact with all necessary elements.  Pay particular attention to users with visual impairments who might be using a screen reader.  Make sure that the focused element is always visible and that any relevant information is conveyed to the user.
