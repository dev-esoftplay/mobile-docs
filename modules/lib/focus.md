```markdown
# LibFocus

This component provides a way to execute functions when a screen gains or loses focus in a React Navigation context. It uses the `@react-navigation/native`'s `useIsFocused` hook and `InteractionManager` to ensure that focus/blur actions are run after interactions are complete.  It also allows you to render a different view when the screen is blurred.

## Installation

This component relies on `@react-navigation/native`. Ensure you have it installed:

```bash
npm install @react-navigation/native
# or
yarn add @react-navigation/native
```

## Usage

```javascript
import { LibFocus } from 'esoftplay/cache/lib/focus/import';

// In your screen component:
function MyScreen() {
  return (
    <LibFocus
      onFocus={() => {
        console.log("Screen is focused!");
        // Perform focus-related actions here
      }}
      onBlur={() => {
        console.log("Screen is blurred!");
        // Perform blur-related actions here
      }}
      blurView={<View style={{flex:1, backgroundColor: 'gray', opacity: 0.5}}/>} // Optional: View to show when blurred
      style={{flex: 1, backgroundColor: 'white'}} // Optional: Style for the focused view
    >
      {/* Content of your screen */}
      <Text>My Screen Content</Text>
    </LibFocus>
  );
}
```

## Props

| Prop        | Type        | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| ----------- | ----------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `isFocused` | `boolean`   | No       | Whether the screen is focused. If not provided, the component uses the `useIsFocused` hook from `@react-navigation/native` to determine the focus state.                                                                                                                                                                                                                                                        |
| `blurView`  | `React.ReactNode` | No       | Optional. A React element to render when the screen is *not* focused. This allows you to display a different view or placeholder when the screen is blurred.                                                                                                                                                                                                                                                                                                                                                        |
| `onFocus`   | `() => void` | No       | Optional. Function to be executed when the screen gains focus.                                                                                                                                                                                                                                                                                                                                                            |
| `onBlur`    | `() => void` | No       | Optional. Function to be executed when the screen loses focus.                                                                                                                                                                                                                                                                                                                                                            |
| `style`     | `ViewStyle` | No       | Optional. Style for the View that wraps the `children` when the screen is focused.                                                                                                                                                                                                                                                                                                                                                      |
| `children`  | `React.ReactNode` | No       | The content to be rendered when the screen is focused.                                                                                                                                                                                                                                                                                                                                                               |

## Functionality

1. **Focus State:** The component uses the `useIsFocused` hook from `@react-navigation/native` to determine if the screen is currently focused.  If the `isFocused` prop is provided, that value overrides the hook.

2. **`useEffect` Hook:** The `useEffect` hook is used to call the `onFocus` or `onBlur` functions whenever the focus state changes.

3. **`InteractionManager`:** The `InteractionManager.runAfterInteractions` function is used to ensure that the `onFocus` and `onBlur` callbacks are executed *after* any ongoing interactions are complete. This is important to avoid potential issues with animations or state updates during transitions.

4. **Conditional Rendering:**
    * If the screen is focused, the `children` prop is rendered (or null if no children). The `style` prop is applied to the wrapping `View`.
    * If the screen is *not* focused and the `blurView` prop is provided, that view is rendered.
    * If the screen is *not* focused, no `blurView` is provided, and a `style` prop is given, an empty `View` with the given style is rendered.
    * If the screen is *not* focused, no `blurView` and no `style` are provided, then `null` is returned (nothing is rendered).

## Important Considerations

* **`@react-navigation/native` Dependency:** This component relies on the `@react-navigation/native` library and its `useIsFocused` hook.  Make sure you have React Navigation set up correctly in your project.
* **`InteractionManager`:** Using `InteractionManager` is crucial for ensuring that focus/blur actions are performed at the correct time, especially when dealing with animations or transitions.
* **Conditional Rendering:** The component's rendering logic allows you to display different content when the screen is focused or blurred.  This can be useful for optimizing performance or providing a different user experience.
* **`blurView` Prop:** The `blurView` prop provides a convenient way to display a placeholder or alternative view when the screen is not focused.
* **`style` Prop:** The `style` prop is applied to the View that wraps the `children` only when the screen is focused.  This allows you to style the *content* of the screen.  If you want to style the `blurView`, apply the style directly to the `blurView` component.
* **Accessibility:** Consider accessibility implications. If the content changes based on the focus state, ensure that users are notified of these changes (e.g., using ARIA attributes or announcements).  If you are showing a `blurView`, make sure it is appropriately labeled and conveys the necessary information to users.
