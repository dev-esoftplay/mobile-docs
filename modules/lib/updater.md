# LibUpdater

This component provides a way to manage application updates using Expo Updates. It includes functionality to check for updates, fetch updates, install updates (reload the app), and display alerts to the user.

## Installation

This component relies on `expo-updates`, `expo-constants`, `esoftplay`. Ensure you have them installed:

```bash
expo install expo-updates expo-constants
npm install esoftplay
# or
yarn add expo-updates expo-constants esoftplay
```

## Usage

```javascript
import { LibUpdater, LibUpdaterProperty } from 'esoftplay/cache/lib/updater/import';

// In your app's main component or a relevant screen:
<LibUpdater show={true} /> {/* Show the update button */}

// Check for updates and prompt the user to install if available:
LibUpdaterProperty.checkAlertInstall(); // Call this function when your app starts or at other appropriate times.

// Or, for more control:
LibUpdaterProperty.check((isNew) => {
  if (isNew) {
    // Show a custom update prompt or perform other actions
    LibUpdaterProperty.alertInstall("Update Available", "A new version of the app is available. Install now?");
    //or without confirmation
    LibUpdaterProperty.install()
  }
});
```

## API

### `LibUpdater` (Component)

Renders a button that triggers the update check and installation process.

### `install(): void`

Installs the available update (reloads the app).

### `alertInstall(title?: string, msg?: string): void`

Displays an alert to the user asking them to install the update.

### `checkAlertInstall(): void`

Checks for updates and, if an update is available, displays the `alertInstall` message.

### `check(callback?: (isNew: boolean) => void): void`

Checks for updates.

*   `callback`: A function that is called with a boolean value indicating whether a new update is available (`true`) or not (`false`).

## Props (`LibUpdater` Component)

| Prop    | Type      | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| ------- | --------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `show`  | `boolean` | Yes      | Controls whether the update button is visible.                                                                                                                                                                                                                                                                                                                                                           |

## Functionality

1.  **Update Check:** The `check` function uses `Updates.checkForUpdateAsync()` to check for available updates.

2.  **Update Fetching:** If an update is available, `Updates.fetchUpdateAsync()` downloads the update.

3.  **Update Installation:** The `install` function uses `Updates.reloadAsync()` to install the update by reloading the app.

4.  **Alerts:** The `alertInstall` function displays an alert to the user, prompting them to install the update.

5.  **Update Button:** The `LibUpdater` component renders a button that, when pressed, triggers the update check and installation process.

6. **Development Mode Handling:** The `check` function skips the update check in development mode (`__DEV__`).

7. **Expo Go Handling:** The update button is hidden if the app is running in Expo Go (`Constants.appOwnership == 'expo'`).  Updates are typically handled differently in Expo Go.

8. **Progress Indicator:** The `LibProgress` component is used to show a progress indicator while checking for updates.

9. **Error Handling:** The `check` function includes a `.catch` block to handle potential errors during the update process.

## Important Considerations

*   **Expo Updates Configuration:** You must configure Expo Updates correctly in your `app.json` or `app.config.js` file.  This includes enabling updates and setting the update URL.  See the Expo documentation for details.
*   **Update URL:**  The update URL is crucial for Expo Updates to work. It points to where your app's update bundles are stored.
*   **Production Builds:** Updates are only relevant for production builds.  In development mode, the app is typically reloaded automatically when changes are made.
*   **Testing:**  Thoroughly test the update process on both iOS and Android devices.
*   **User Experience:**  Provide clear and informative messages to the user during the update process.
*   **Error Handling:**  Implement proper error handling to gracefully handle situations where the update check or installation fails.
*   **`esoftplay` Dependencies:**  The component uses `esoftplay`'s `LibIcon`, `LibProgress`, and `LibStyle` components.  Make sure these are available in your project.
*   **`createTimeout`:**  The `createTimeout` function from `esoftplay` is used to introduce a small delay before reloading the app after an update is fetched. This might be necessary to ensure that the update is fully downloaded and ready to be installed.
*   **Language Support:** The component uses `esp.lang` for localized strings.  Make sure you have your language files configured correctly.
*   **Button Visibility:** The `show` prop controls the visibility of the update button.  You can use this to show or hide the button based on your app's logic (e.g., only show it when an update is available or only show it in certain screens).
*   **Pressable Component:** The update button uses `Pressable` which is more flexible than `TouchableOpacity`.
*   **Elevation:** The update button uses `LibStyle.elevation` to add a shadow effect.
*   **Conditional Rendering:** The update button is only rendered if `props.show` is true and Expo Updates are enabled in the `app.json` configuration.  This prevents the button from being shown unnecessarily.
* **Alert Messages:** The alert messages are localized using `esp.lang`.  This makes it easy to support multiple languages in your app.
* **Development Mode Behavior:** The update check is skipped in development mode (`__DEV__`). This is because updates are typically handled differently in development.
* **Expo Go Handling:** The update button is hidden when the app is running in Expo Go.  This is because updates are handled differently in Expo Go.
* **Progress Indicator:** The `LibProgress` component is used to provide visual feedback to the user while checking for updates. This improves the user experience.
