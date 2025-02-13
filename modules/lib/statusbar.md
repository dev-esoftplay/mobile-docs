# LibStatusbar

This component provides a simple way to manage the status bar in your Expo React Native application. It wraps the `StatusBar` component from `expo-status-bar` and allows you to easily set the style and background color.

## Installation

This component relies on `expo-status-bar`. Ensure you have it installed:

```bash
expo install expo-status-bar
# or
yarn add expo-status-bar
```

## Usage

```javascript
import { LibStatusbar } from 'esoftplay/cache/lib/statusbar/import';

// Light status bar with a custom background color:
<LibStatusbar style="light" backgroundColor="blue" />

// Dark status bar (default background color):
<LibStatusbar style="dark" />

// In your root component or a high-level component:
<View style={{ flex: 1 }}>
  <LibStatusbar style="light" backgroundColor="red" /> {/* Example usage */}
  {/* Rest of your app content */}
</View>
```

## Props

| Prop            | Type                 | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| --------------- | -------------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `style`         | `"dark" \| "light"` | Yes      | The style of the status bar.  `"dark"` for light text and icons, `"light"` for dark text and icons.                                                                                                                                                                                                                                                                                                                                                                  |
| `backgroundColor` | `string`             | No       | The background color of the status bar.  If not provided, the default platform-specific background color is used.                                                                                                                                                                                                                                                                                                                                                        |

## Functionality

This component is a simple wrapper around the `StatusBar` component from `expo-status-bar`. It sets the `translucent` prop to `true` and passes the `style` and `backgroundColor` props directly to the `StatusBar` component.

## Important Considerations

*   **`expo-status-bar` Dependency:** Ensure that `expo-status-bar` is correctly installed.
*   **Translucent Status Bar:** The `translucent` prop is always set to `true`.  This means that your app's content will appear behind the status bar.  You'll need to adjust your app's layout to account for the status bar height.  You can use `SafeAreaView` to ensure your content is not obscured by the status bar.
*   **Platform Differences:** The appearance and behavior of the status bar can vary slightly between platforms (iOS and Android).  `expo-status-bar` handles most of these differences, but you might need to make some platform-specific adjustments in your styling.
*   **Styling:**  You can customize the appearance of the status bar using the `style` and `backgroundColor` props.  For more advanced customization, you might need to use platform-specific code.
*   **SafeAreaView:** It's highly recommended to use `SafeAreaView` to wrap your app's content to ensure it is not obscured by the status bar (or other system UI elements like notches).  This is especially important when using a translucent status bar.
* **Expo Configuration:**  If you're using Expo, you might need to configure the status bar behavior in your `app.json` or `app.config.js` file, especially if you are building standalone apps.  Refer to the Expo documentation for more details.  This component simplifies the runtime management of the status bar, but the base configuration might still be needed in your Expo config file.
* **Component Placement:** It is recommended to place the `LibStatusbar` component high in your component tree (e.g., in your root component) to ensure that the status bar is configured correctly throughout your application.  This also allows you to easily manage the status bar's appearance from a central location.
