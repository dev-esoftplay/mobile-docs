# LibSlidingup

This component implements a sliding up panel or bottom sheet that can be used to display additional content. It supports showing and hiding the panel with animation, handles back button presses to close the panel, and integrates with the keyboard avoiding view.

## Installation

This component relies on `esoftplay`. Ensure you have it installed:

```bash
npm install esoftplay
# or
yarn add esoftplay
```

## Usage

```javascript
import { LibSlidingup } from 'esoftplay/cache/lib/slidingup/import';

// In your parent component:
const [showSlidingPanel, setShowSlidingPanel] = useState(false);

<LibSlidingup onChangeShow={(isShow) => setShowSlidingPanel(isShow)}>
  {/* Content of the sliding panel */}
  <View style={{ height: 200, backgroundColor: 'white' }}>
    <Text>Sliding Panel Content</Text>
  </View>
</LibSlidingup>

// To show the panel:
<Button title="Show Panel" onPress={() => setShowSlidingPanel(true)} />

// Or, using the component's methods directly (recommended way):
const slidingUpRef = useRef<LibSlidingup>(null);
<LibSlidingup ref={slidingUpRef}>
  {/* ... */}
</LibSlidingup>

<Button title="Show Panel" onPress={() => slidingUpRef.current?.show()} />
<Button title="Hide Panel" onPress={() => slidingUpRef.current?.hide()} />

```

## Props

| Prop          | Type        | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| ------------- | ----------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `onChangeShow` | `(isShow: boolean) => void` | No       | Callback function called when the visibility of the sliding panel changes. Receives a boolean value indicating whether the panel is shown (`true`) or hidden (`false`).                                                                                                                                                                                                                                                                                                                                                       |
| `children`    | `any`       | No       | The content to be displayed within the sliding panel.                                                                                                                                                                                                                                                                                                                                                     |

## API (Methods)

*   `show(): void`: Shows the sliding up panel.
*   `hide(): void`: Hides the sliding up panel.

## Functionality

1. **Animated Sliding:** The component uses `Animated` to create a smooth sliding animation for the panel.

2. **Modal Overlay:** The sliding panel is presented as a modal overlay, covering the entire screen. A semi-transparent backdrop is used behind the panel.

3. **Back Button Handling:** The component listens for back button presses and closes the panel when the back button is pressed.

4. **Keyboard Avoiding View:** The panel is wrapped in a `LibKeyboard_avoid` component to handle keyboard interactions. This prevents the keyboard from obscuring the panel content when it slides up.

5. **`onChangeShow` Callback:**  The `onChangeShow` callback provides a way for parent components to be notified of the sliding panel's visibility changes.

## Important Considerations

*   **`esoftplay` Dependencies:** This component relies on `esoftplay` for component base class, keyboard avoiding view and style utilities.  Make sure `esoftplay` is configured in your project.
*   **Keyboard Handling:** The integration with `LibKeyboard_avoid` helps to prevent the keyboard from covering the panel's content.
*   **Back Button Handling:** The component listens for back button presses and automatically closes the panel.
*   **Animation:** The animation duration can be customized.
*   **Styling:** The appearance of the panel and the backdrop can be customized via styles.
*   **Z-Index:** The sliding panel has a high `zIndex` to ensure it appears above other content.
*   **Accessibility:** Consider accessibility. Use appropriate ARIA attributes (e.g., `aria-modal`, `aria-label`) to make the sliding panel accessible to users with visual impairments.  Provide a way to dismiss the panel using the keyboard.
* **Component Lifecycle**: The `componentDidUpdate` lifecycle method is used to add and remove the back handler event listener. It's crucial to remove the listener when the panel is hidden to prevent memory leaks and unexpected behavior.
* **Ref usage**: You can control the `LibSlidingup` component from a parent component by using a ref.  This allows you to call the `show()` and `hide()` methods directly.
