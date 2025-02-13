# LibToast

This component provides a toast notification functionality. It displays a message briefly at the top of the screen and automatically hides after a specified timeout.  It uses `react-native-reanimated` for smooth animations.

## Installation

This component relies on `esoftplay` and `react-native-reanimated`. Ensure you have them installed:

```bash
npm install esoftplay react-native-reanimated
# or
yarn add esoftplay react-native-reanimated
```

You'll also need to configure `react-native-reanimated` if you haven't already. Follow the instructions in the library's documentation.

## Usage

```javascript
import { LibToastProperty } from 'esoftplay/cache/lib/toast/import';

// Displaying a toast message:
LibToastProperty.show("This is a toast message!"); // Default timeout of 3 seconds
LibToastProperty.show("This message will be shown for 5 seconds.", 5000);

// Hiding the toast manually (usually not necessary):
LibToastProperty.hide();

// The component should be rendered somewhere in your app (e.g., in your root component):
<LibToast />
```

## API

### `LibToast` (Component)

Renders the toast notification. It should be placed somewhere high in your component tree (like your app's main component) so that it can overlay other content.

### `show(message: string, timeout?: number): void`

Displays the toast message.

*   `message`: The message to display.
*   `timeout`: Optional. The duration in milliseconds before the toast automatically hides. Defaults to 3000ms.

### `hide(): void`

Hides the toast message.  Generally, you won't need to call this manually as the toast hides automatically.

## Props (`LibToast` Component)

This component doesn't accept any props directly.  The message and timeout are passed to the `show()` function.

## Functionality

1.  **Global State:** The component uses `useGlobalState` from `esoftplay` to manage the toast message and timeout.

2.  **Animated Appearance:** The toast uses `react-native-reanimated` for a smooth, animated appearance (slide down and fade in).

3.  **Automatic Hiding:** The toast automatically hides after the specified timeout using `setTimeout`.

4.  **Manual Hiding:** The `hide()` function can be used to manually hide the toast.

5.  **Message Display:** The toast displays the provided message in a styled `Text` component.

6. **Preventing Multiple Timers:**  The code includes logic to clear any existing timeouts before setting a new one.  This prevents multiple toasts from overlapping and ensures that only one timeout is active at a time.

7. **First Render Handling:** The `isFirstInit` ref is used to prevent the animation from running on the initial render of the component.  This ensures that the toast doesn't animate in before it has a chance to render.

## Important Considerations

*   **`esoftplay` Dependency:** This component relies on `esoftplay` for global state management.
*   **`react-native-reanimated` Dependency:**  The component uses `react-native-reanimated` for animations.
*   **Global State Management:** The use of global state makes it easy to show the toast from any part of your application.
*   **Timeout Handling:**  The component handles timeouts correctly to ensure that the toast hides automatically.  The clearing of existing timeouts prevents issues when multiple toasts are shown quickly.
*   **Styling:** The appearance of the toast can be customized by modifying the styles applied to the `View` and `Text` components.
*   **Positioning:** The toast is positioned at the top of the screen (below the status bar) using absolute positioning.  You might need to adjust the positioning based on your app's layout.
*   **Accessibility:** Consider accessibility. Use appropriate ARIA attributes to indicate the toast message to users with visual impairments.  For example, you could use `aria-live="polite"` or `aria-live="assertive"` (depending on the importance of the message) on the toast container.
* **Reduce Motion:** The `reduceMotion` option in `withTiming` is set to `ReduceMotion.Never`.  This means the animation will always play, even if the user has reduced motion settings enabled on their device.  Consider setting this to `ReduceMotion.IfUserPrefers` to respect the user's motion preferences.
* **`useAnimatedProps`:** The `useAnimatedProps` hook is used to optimize the animation by directly updating the view's properties on the native thread.  This is generally more performant than using `useAnimatedStyle` for animations that involve transforms and opacity.
* **String Conversion:** The `String()` function is used to ensure that the message is always treated as a string, even if it's a number or other data type.  This prevents potential errors.
