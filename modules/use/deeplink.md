# UseDeeplink

This custom hook handles deep linking in a React Native application. It listens for incoming URLs, processes them, and navigates to the appropriate screen based on the deep link data.

## Installation

This hook requires `esoftplay`. Ensure it's installed:

```bash
npm install esoftplay
# or
yarn add esoftplay
```

## Usage

```javascript
import { UseDeeplink } from 'esoftplay/cache/use/deeplink/import';

function MyComponent() {
  UseDeeplink(); // Optional: Pass a default URL

  //get deeplink params
  const [deeplinkParams] = UseDeeplinkProperty.params.useState()

}
```

## API

### `UseDeeplink(defaultUrl?: string): void`

This custom hook takes an optional `defaultUrl` as an argument.

## Parameters

*   `defaultUrl`: (Optional) The default URL to check against when processing deep links.

## Functionality

1.  **URL Handling:** The hook listens for incoming URLs using `Linking.getInitialURL()` (for initial app launch) and `Linking.addEventListener('url', ...)` (for subsequent deep links).

2.  **URL Processing:** The `doLink` function processes the URL:
    *   Checks if the URL includes the `defaultUrl` or the app's domain.
    *   Replaces a portion of the URL to point to a "deeplink/" path.
    *   Ensures the URL starts with the correct protocol.
    *   Removes trailing dots from the URL.

3.  **API Call:** Makes a custom API call using `LibCurl` to the processed URL.

4.  **Navigation:** Based on the API response (`res.result.module` and `res.result.url`), navigates to the specified screen using `LibNavigation.push`.  Handles the case where `LibNavigation` might not be immediately ready.

5.  **Error Handling:** Displays an alert if the API response indicates an error.

6.  **Debouncing:** Uses `esp.mod("lib/utils").debounce` to prevent rapid, repeated API calls when multiple deep links are received quickly.

7.  **Cleanup:** Removes the URL event listener when the component unmounts.

## Important Considerations

*   **`esoftplay` Dependencies:** Relies on `esoftplay`'s `esp`, `LibCurl`, and `LibNavigation`.
*   **Deep Link Configuration:** You'll need to configure deep linking in your app (both native and potentially web-based).
*   **API Endpoint:** This hook assumes an API endpoint that handles the processed deep link URL and returns a JSON response with `module` and `url` properties.
*   **Navigation:** The hook uses `LibNavigation.push` to navigate.  Ensure `LibNavigation` is correctly set up.
*   **Error Handling:** Basic error handling is provided with an alert.  Consider more robust error handling in a production app.
*   **Debouncing:** The debounce mechanism prevents excessive API calls. Adjust the debounce time (500ms) as needed.
*   **URL Manipulation:** The hook manipulates the URL, adding "deeplink/" and ensuring the correct protocol.  Make sure this logic matches your deep link structure.
*   **`useCallback`:** The `doLink` function is wrapped in `useCallback` to prevent unnecessary re-creations of the function.
*   **`useEffect`:** The `useEffect` hook sets up the deep link listeners and cleans them up when the component unmounts.
*   **`Linking`:** The hook uses `Linking` to handle incoming URLs.
*   **Default URL:** The `defaultUrl` parameter allows you to specify a URL to check against.
*   **Navigation Readiness:** The hook checks if `LibNavigation` is ready before attempting to navigate. This is important to prevent errors if the navigation system is not yet initialized.
* **Trailing Dot Removal:** The `removeLastDot` function removes trailing dots from the URL. This might be necessary if your deep links sometimes include trailing dots that need to be removed before making the API request.  This is a good practice to ensure that the URL is consistent and correctly processed.
* **Protocol Handling:** The code ensures the URL starts with the correct protocol (`protocol://`). This is important because deep links might sometimes be missing the protocol.
* **Error Message Localization:** The error message displayed in the alert is localized using `esp.lang`. This is a good practice for supporting multiple languages in your app.