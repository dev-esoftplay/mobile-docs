# LibProgress

This component displays a modal progress indicator with an optional message. It provides methods to show and hide the indicator, and handles back button presses to dismiss it.  It uses global state to manage the visibility and message.

## Installation

This component relies on `esoftplay`. Ensure you have it installed:

```bash
npm install esoftplay
# or
yarn add esoftplay
```

## Usage

```javascript
import { LibProgress } from 'esoftplay/cache/lib/progress/import';

// Show the progress indicator:
LibProgress.show("Loading...");

// Hide the progress indicator:
LibProgress.hide();

// The component should be rendered somewhere in your app (e.g., in your root component):
<LibProgress />
```

## API

### `LibProgress` (Component)

Renders the progress modal.  It should be placed somewhere high in your component tree (like your app's main component) so that it can overlay other content.

### `LibProgress.show(message?: string)`

Shows the progress indicator with an optional message.

### `LibProgress.hide()`

Hides the progress indicator.

## Props (`LibProgress` Component)

| Prop      | Type    | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| --------- | ------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `show`    | `boolean` | No       | Controls the visibility of the progress indicator.  This prop is typically managed internally by the component using global state.  You usually won't need to set this prop directly.                                                                                                                                                                                                                                                                                                                                                        |
| `message` | `string`  | No       | The message to display below the progress indicator.                                                                                                                                                                                                                                                                                                                                                        |

## Functionality

1. **Global State:** The component uses `useGlobalState` from `esoftplay` to manage the visibility (`show`) and message of the progress indicator.  This allows you to control the progress indicator from anywhere in your application.

2. **Modal Overlay:** The progress indicator is displayed as a modal overlay, covering the entire screen with a semi-transparent background.

3. **Message Display:** The optional `message` prop is displayed below the `ActivityIndicator`.

4. **Back Button Handling:** The component listens for back button presses (both hardware and software) and hides the progress indicator when the back button is pressed.  This provides a convenient way for users to dismiss the progress indicator.

5. **Styling:** The component uses styles from `esoftplay`'s `LibTheme` and `LibStyle` for consistent styling.

## Important Considerations

*   **`esoftplay` Dependency:** This component relies on `esoftplay` for global state management, styling, and text styling.
*   **Global State Management:** The use of global state makes it easy to control the progress indicator from any part of your application.
*   **Back Button Handling:** The component automatically handles back button presses to dismiss the progress indicator.
*   **Styling:** The appearance of the progress indicator can be customized by modifying the styles in `LibTheme` and `LibStyle`.
*   **Placement:** The `LibProgress` component should be rendered somewhere high in your component tree (e.g., in your root component) so that it can overlay other content.
*   **Accessibility:** Consider accessibility.  Use appropriate ARIA attributes to indicate the loading state to users with visual impairments.  For example, you could set `aria-busy={true}` on the overlay view.  Provide an `accessibilityLabel` for the progress indicator.  Since the overlay covers the entire screen, ensure that any content underneath it is not interactable while the progress indicator is visible.
*   **Interaction Blocking:** The modal overlay blocks user interaction with the underlying content while the progress indicator is shown.  This is the standard behavior for a modal progress indicator.
* **`BackHandler`:** The component uses `BackHandler` to listen for back button presses.  Be aware that `BackHandler` can be tricky to manage in complex navigation scenarios.  If you have a complex navigation structure, you might need to adjust the back button handling logic.  It's very important to remove the event listener in the `else` block (when `show` is false) to prevent memory leaks and unexpected behavior.
