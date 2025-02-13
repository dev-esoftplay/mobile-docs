# LibVersion

This component displays a screen informing the user about a new app version and provides an option to update. It checks for updates, displays update prompts, and handles navigation based on the update status.

## Installation

This component relies on `esoftplay`, `expo-constants`. Ensure you have them installed:

```bash
npm install esoftplay expo-constants
# or
yarn add esoftplay expo-constants
```

## Usage

```javascript
import { LibVersion } from 'esoftplay/cache/lib/version/import';

// Check for updates (call this when your app starts or at appropriate times):
LibVersion.check();

// The LibVersion component is usually navigated to when an update is available:
LibNavigation.replace("lib/version", { res: updateResponseData, msg: updateMessage });

// Example of updateResponseData:
const updateResponseData = {
  title: "New Update Available",
  version: "1.2.0",
  android: "https://play.google.com/your-app",
  ios: "https://itunes.apple.com/your-app",
  version_publish: 2,
};

// Example of updateMessage:
const updateMessage = "A new version with exciting features is available!";

// In your navigation configuration:
// ...
```

## API

### `LibVersion` (Component)

Renders the update information screen.

### `appVersion(): string` (Static Method)

Returns the current app version.

### `showDialog(title: string, message: string, link: string, onOk: (link: string) => void, onCancel: () => void): void` (Static Method)

Displays a confirmation dialog for updating.

### `closeApp(): void` (Static Method)

Closes the app (platform-specific).

### `toStore(link: string): void` (Static Method)

Opens the app store link.

### `onDone(res: any, msg: string): void` (Static Method)

Handles the response from the version check API.

### `check(): void` (Static Method)

Checks for updates using the `public_version` API endpoint.

## Props (`LibVersion` Component)

This component receives navigation parameters:

*   `res`: An object containing update information (title, version, Android link, iOS link, version\_publish).
*   `msg`: A message to display to the user.

## Functionality

1.  **Version Check:** The `check` function fetches version information from the `public_version` API endpoint.

2.  **Dialog:** The `showDialog` function displays a confirmation dialog prompting the user to update.

3.  **App Store Link:** The `toStore` function opens the appropriate app store link (Android or iOS) based on the platform.

4.  **App Close:** The `closeApp` function closes the app.

5.  **Update Logic:** The `onDone` function compares the current app version with the available version. If an update is available, it navigates to the `LibVersion` component. If the `version_publish` is higher than the `publish_id` in the config and not in development mode, it navigates to the `lib/version_view` component.

6.  **Update Screen:** The `LibVersion` component displays the update information (title, message) and provides a button to open the app store link.

## Important Considerations

*   **`public_version` API:** This component relies on a `public_version` API endpoint that returns version information.  You'll need to implement this API endpoint.
*   **Version Comparison:** The component compares versions as strings.  For more robust version comparison, consider using a dedicated version comparison library.
*   **Platform-Specific Links:** The component uses different app store links for Android and iOS.
*   **Navigation:** The component uses `LibNavigation` for navigation.
*   **Dialog:** The component uses `LibDialog` for displaying the update confirmation dialog.
*   **Styling:** The component uses `LibStyle` and `LibTextstyle` for styling.
*   **Asset:** Uses `splash.png` from assets.
*   **Localization:** Uses `esp.lang` for localized strings.
*   **Development Mode:** The component behaves differently in development mode.
*   **`LibUpdaterProperty`:** The component uses `LibUpdaterProperty` to handle updates when `version_publish` is higher.  This suggests a more complex update flow that might involve downloading and installing updates within the app.  Make sure `LibUpdaterProperty` is correctly implemented and configured.
* **Navigation Parameters:**  The `LibVersion` component relies on navigation parameters to receive the update information.  Ensure that these parameters are passed correctly when navigating to this component.
* **Version Numbering Scheme:** The `isAvailableNewVersion` function compares versions as strings.  This might not be suitable for all versioning schemes (e.g., semantic versioning).  Consider using a version comparison library if you need more sophisticated version comparison.
* **User Experience:**  Provide clear and informative messages to the user throughout the update process.  Consider adding a progress indicator during the download and installation phases.
* **Error Handling:**  Implement proper error handling to gracefully handle situations where the API call fails or the update process encounters an error.
* **Deep Linking:**  The component uses deep linking to open the app store links.  Make sure deep linking is configured correctly in your app.