# LibVideo

This component displays a video using a `WebView`. It's designed to embed YouTube videos and handles cases where the video code is missing.

## Installation

This component relies on `react-native-webview` and `esoftplay`. Ensure you have them installed:

```bash
npm install react-native-webview esoftplay
# or
yarn add react-native-webview esoftplay
```

## Usage

```javascript
import { LibVideo } from 'esoftplay/cache/lib/video/import';

<LibVideo code="YOUR_YOUTUBE_VIDEO_ID" style={{ height: 200 }} />
```

## Props

| Prop    | Type      | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| ------- | --------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `code`  | `string`  | Yes      | The YouTube video ID.                                                                                                                                                                                                                                                                                                                                                                                         |
| `style` | `ViewStyle` | No       | Additional styles to apply to the `WebView` container.                                                                                                                                                                                                                                                                                                                                                      |

## Functionality

1.  **WebView:** The component uses a `WebView` to embed the YouTube video.

2.  **Video ID:** The `code` prop provides the YouTube video ID, which is used to construct the video URL.

3.  **Loading Indicator:** An `ActivityIndicator` is displayed while the video is loading.

4.  **Missing Code Handling:** If the `code` prop is missing, a toast message is displayed using `LibToastProperty`.

5.  **Styling:** The `style` prop allows you to customize the dimensions and other styles of the video player.  The component sets a default aspect ratio of 16:9 based on the screen width.

6. **YouTube URL Construction:** The component constructs the YouTube embed URL using the provided `code` and includes several parameters:
    *   `rel=0`: Prevents related videos from being shown at the end.
    *   `autoplay=0`: Disables autoplay.
    *   `showinfo=0`: Hides video title and uploader info.
    *   `controls=1`: Shows video controls.
    *   `modestbranding=1`: Uses a more subtle YouTube branding.

## Important Considerations

*   **`react-native-webview`:** This component relies on `react-native-webview` for displaying the video.  Make sure it's installed and configured correctly.
*   **YouTube Video ID:**  Ensure that you provide the correct YouTube video ID in the `code` prop.
*   **Missing Code Handling:** The component displays a toast message if the `code` prop is missing.  Make sure you have `LibToastProperty` correctly configured in your project.
*   **Styling:**  The component provides a default 16:9 aspect ratio.  You can customize this using the `style` prop.  It's important to set the `width` and `height` (or just `width` with the correct aspect ratio) in the styles to ensure the video displays correctly.
*   **Loading Indicator:** The `ActivityIndicator` provides visual feedback to the user while the video is loading.
*   **`useWebKit`:** The `useWebKit` prop is set to `true`. This is often necessary for better performance and compatibility, especially on iOS.
*   **JavaScript Enabled:** JavaScript is enabled in the `WebView` (`javaScriptEnabled={true}`). This is required for the YouTube player to function correctly.
*   **Error Handling:** Consider adding more robust error handling to the component.  For example, you could handle cases where the video fails to load or the network connection is lost.
*   **Localization:** The toast message for the missing code is localized using `esp.lang`.  Make sure you have the necessary language files configured.
* **Aspect Ratio:**  The component calculates the video height based on a 16:9 aspect ratio and the screen width.  This ensures that the video maintains its aspect ratio and scales appropriately to different screen sizes.  This is a good practice for video players.
* **Styling with `StyleSheet.flatten`:** The component uses `StyleSheet.flatten` to merge the default styles with any styles provided in the `style` prop.  This is important because the `style` prop can accept either a style object or an array of style objects. `StyleSheet.flatten` ensures that the styles are correctly merged into a single style object.
