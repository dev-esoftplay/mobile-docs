# LibVersion_view

This component displays a splash screen with a message and an activity indicator, and then automatically reloads the application using Expo Updates after a 15-second timeout.  It's typically used during the application update process.

## Installation

This component relies on `expo-updates` and `esoftplay`. Ensure you have them installed:

```bash
expo install expo-updates
npm install esoftplay
# or
yarn add expo-updates esoftplay
```

## Usage

```javascript
import { LibVersion_view } from 'esoftplay/cache/lib/version_view/import';

// In your app's main component or a relevant screen:
<LibVersion_view />
```

## API

### `LibVersion_view` (Component)

Renders the splash screen.

## Props

This component does not take any props.

## Functionality

1.  **Splash Screen:** The component displays a splash screen using an `ImageBackground` component. The image source is retrieved from `esp.assets('splash.png')`.

2.  **Message:** A message is displayed in the center of the screen using the `LibTextstyle` component. The message text is retrieved using `esp.lang("lib/version_view", "msg")`, allowing for localization.

3.  **Activity Indicator:** An `ActivityIndicator` is shown to indicate that the app is loading or updating.

4.  **Automatic Reload:** The `useEffect` hook and the `useTimeout` hook from `esoftplay` are used to trigger an app reload after a 15-second timeout.  This is done using `Updates.reloadAsync()`.

## Important Considerations

*   **Expo Updates Configuration:** You must configure Expo Updates correctly in your `app.json` or `app.config.js` file for `Updates.reloadAsync()` to work.  This usually involves setting the update URL. Refer to the Expo Updates documentation for more information.
*   **Splash Screen Image:** Make sure you have a `splash.png` image in your assets folder, as this is used as the background of the splash screen.
*   **Localization:** The message text is retrieved using `esp.lang()`, so you can easily localize the message by providing translations in your language files.
*   **Timeout:** The app reload is triggered after a 15-second timeout. You can adjust this timeout if needed.
*   **Component Lifecycle:** The `useEffect` hook ensures that the timeout is set only once when the component mounts.
*   **`esoftplay` Dependencies:**  The component uses `esoftplay`'s `LibTextstyle` and `useTimeout` hooks.  Make sure these are available in your project.
*   **Image Background:** The `ImageBackground` component is used to display the splash screen image.  Make sure the image is correctly placed in your assets folder.
*   **Activity Indicator:** The `ActivityIndicator` provides visual feedback to the user while the app is loading or updating.
* **Navigation:** This component is designed to be displayed while the app is loading or updating, so it likely won't be part of your regular navigation flow. You might display it briefly at app startup or when an update is being installed.
* **Error Handling:**  Consider adding error handling, especially around `Updates.reloadAsync()`.  While a reload failure is unlikely, it's good practice to handle potential errors gracefully.
* **User Experience:**  Provide clear instructions or feedback to the user if the update process fails or takes longer than expected.  A 15-second delay before the reload might be a bit long; you could consider shortening it or making it configurable.
* **Testing:** Test this component thoroughly on both iOS and Android devices to ensure that the splash screen is displayed correctly and that the app reloads as expected.
