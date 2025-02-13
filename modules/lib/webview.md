```markdown
# LibWebview

This component provides a customizable WebView for displaying web content. It supports dynamic height adjustment based on the content, animations, and handling of external links.

## Installation

This component relies on `react-native-webview` and `esoftplay`. Ensure you have them installed:

```bash
npm install react-native-webview esoftplay
# or
yarn add react-native-webview esoftplay
```

## Usage

```javascript
import { LibWebview } from 'esoftplay/cache/lib/webview/import';

<LibWebview
  source={{ uri: 'https://www.example.com' }}
  defaultHeight={200}
  needAnimate={true}
  onMessage={(event) => console.log('Message from WebView:', event.nativeEvent.data)}
  onFinishLoad={() => console.log("WebView loaded")}
/>

// Or, using HTML content:
<LibWebview
  source={{ html: '<p>Some HTML content</p>' }}
  defaultHeight={100}
/>
```

## Props

| Prop                               | Type                                     | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| ---------------------------------- | ---------------------------------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `source`                           | `LibWebviewSourceProps`                  | Yes      | The source of the WebView content. Can be either a `uri` (URL) or `html` (HTML string).                                                                                                                                                                                                                                                                                                                                                        |
| `defaultHeight`                    | `number`                                 | No       | The initial height of the WebView before the content height is determined. Defaults to 100.                                                                                                                                                                                                                                                                                                                                                      |
| `needAnimate`                      | `boolean`                                | No       | Whether to animate the WebView's height change. Defaults to `true`.                                                                                                                                                                                                                                                                                                                                                                       |
| `AnimationDuration`                | `number`                                 | No       | The duration of the height change animation in milliseconds. Defaults to 500.                                                                                                                                                                                                                                                                                                                                                         |
| `needAutoResetHeight`              | `boolean`                                | No       | Whether to automatically adjust the WebView's height based on the content. Defaults to `true`.                                                                                                                                                                                                                                                                                                                                                      |
| `maxWidth`                         | `number`                                 | No       | The maximum width of the WebView. If provided, the height will be adjusted to maintain the aspect ratio of the content.                                                                                                                                                                                                                                                                                                                               |
| `onMessage`                        | `(event: any) => void`                   | No       | Callback function called when the WebView sends a message (e.g., to communicate its content height).                                                                                                                                                                                                                                                                                                                                                      |
| `bounces`                          | `boolean`                                | No       | Whether the WebView should bounce when scrolling reaches the edge. Defaults to `true`.                                                                                                                                                                                                                                                                                                                                                           |
| `onLoadEnd`                        | `() => void`                             | No       | Callback function called when the WebView finishes loading.                                                                                                                                                                                                                                                                                                                                                                                    |
| `style`                            | `ViewStyle`                              | No       | Additional styles to apply to the WebView.                                                                                                                                                                                                                                                                                                                                                                                    |
| `scrollEnabled`                    | `boolean`                                | No       | Whether scrolling is enabled within the WebView. Defaults to `false`.                                                                                                                                                                                                                                                                                                                                                                        |
| `automaticallyAdjustContentInsets` | `boolean`                                | No       | A Boolean value that determines whether the web view automatically adjusts its content insets. Defaults to `true`.                                                                                                                                                                                                                                                                                                                                                      |
| `scalesPageToFit`                  | `boolean`                                | No       | A Boolean value that determines whether the web view scales the page content to fit. Defaults to `true`.                                                                                                                                                                                                                                                                                                                                                        |
| `onFinishLoad`                     | `() => void`                             | Yes       | A callback function that is called when the WebView finishes loading the page.                                                                                                                                                                                                                                                                                                                                                                                    |

### `LibWebviewSourceProps`

```typescript
interface LibWebviewSourceProps {
  uri?: string,
  html?: string
}
```

## Functionality

1.  **Dynamic Height Adjustment:** The component dynamically adjusts the WebView's height based on the content using messages passed from the WebView.  The WebView injects JavaScript code to send the `scrollWidth` and `scrollHeight` of the content.  The component then calculates the appropriate height, taking into account `maxWidth` if provided.

2.  **HTML Wrapping:** If the `source` prop contains `html`, it wraps the HTML content with `webviewOpen` and `webviewClose` from the `esp.config()`.  This allows for consistent styling or injection of JavaScript into the WebView content.

3.  **Animation:** The component animates the height change using `Animated.timing`.

4.  **External Link Handling:** The component intercepts navigation events and opens external links in the device's default browser.

5.  **Loading:** The component calls `onFinishLoad` when the WebView finishes loading.

## Important Considerations

*   **`react-native-webview`:** This component relies on `react-native-webview`. Ensure it's installed and correctly configured.
*   **Message Passing:** The component uses message passing between the WebView and the React Native code to determine the content height.  Make sure the injected JavaScript code is correct.
*   **HTML Wrapping:** The HTML wrapping functionality allows you to inject custom JavaScript or CSS into the WebView content.
*   **Animation:** The height change is animated.  You can disable this by setting `needAnimate` to `false`.
*   **External Links:** The component handles external links by opening them in the default browser.
*   **Height Calculation:** The height calculation takes into account `maxWidth` to maintain the aspect ratio if a maximum width is specified.  It also makes a platform-specific adjustment for iOS.
*   **`esp.config()`:** The component uses `esp.config()` to get the `webviewOpen` and `webviewClose` strings for HTML wrapping.  Make sure these are defined in your `esp.config`.
*   **`onLoadEnd` vs. `onFinishLoad`:** The component uses both `onLoadEnd` (from `react-native-webview`) and the custom `onFinishLoad` prop.  `onLoadEnd` is a standard WebView callback, while `onFinishLoad` is a custom prop that's specifically called after a small delay. This delay is likely implemented to ensure that the WebView's content is fully loaded and its dimensions are correctly reported before the `onFinishLoad` callback is executed.
* **`Animated.View` and `overflow: "hidden"`:** The WebView is wrapped in an `Animated.View` with `overflow: "hidden"`.  This is essential for the height animation to work correctly.  The `overflow: "hidden"` style clips the content of the WebView during the animation, preventing it from overflowing the container.
* **`injectedJavaScript`:** The `injectedJavaScript` prop is used to inject JavaScript code into the WebView.  This code sends the `scrollWidth` and `scrollHeight` of the document body to the React Native side, allowing the component to dynamically adjust its height.  This is a common technique for handling dynamic content height in WebViews.
* **`onNavigationStateChange`:** This prop is used to intercept navigation events within the WebView.  When the user clicks on a link that should open in an external browser, the `onNavigationStateChange` handler stops the WebView from loading the link and instead opens it using `Linking.openURL`.  This is a crucial part of handling external links correctly.
* **`ref`:** The component uses a `ref` to access the `WebView` instance.  This allows the component to call methods on the `WebView`, such as `stopLoading`, in the `onNavigationStateChange` handler.
