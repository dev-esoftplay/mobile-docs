# LibSociallogin

This component provides a WebView-based social login functionality. It loads a specified URL (presumably a social login provider's page), handles the response (user data) received via WebView messages, and stores the user data using AsyncStorage.

## Installation

This component relies on `@react-native-async-storage/async-storage` and `react-native-webview` and `esoftplay`. Ensure you have them installed:

```bash
npm install @react-native-async-storage/async-storage react-native-webview esoftplay
# or
yarn add @react-native-async-storage/async-storage react-native-webview esoftplay
```

## Usage

```javascript
import { LibSociallogin } from 'esoftplay/cache/lib/sociallogin/import';

// In a component where you initiate the social login:
<LibSociallogin
  url="[https://example.com/social-login](https://www.google.com/search?q=https://example.com/social-login)" // URL of the social login provider
  onResult={(userData) => {
    console.log("User data received:", userData);
    // Do something with the user data (e.g., navigate, update state)
  }}
/>

// Or, passing the URL as a navigation parameter:
LibNavigation.navigate('SocialLoginScreen', { url: '[https://example.com/social-login](https://www.google.com/search?q=https://example.com/social-login)' });

// Accessing stored user data:
LibSociallogin.getUser().then(userData => {
  if (userData) {
    console.log("Stored user data:", userData);
  }
});

// Removing stored user data:
LibSociallogin.delUser();
```

## API

### `LibSociallogin` (Component)

Renders the WebView for social login.

### Static Methods

*   `LibSociallogin.setUser(userData: any): void`: Stores user data in AsyncStorage.
*   `LibSociallogin.delUser(): void`: Removes user data from AsyncStorage.
*   `LibSociallogin.getUser(callback?: (userData: any) => void): Promise<any>`: Retrieves user data from AsyncStorage.  You can use a callback or a Promise.

## Props (`LibSociallogin` Component)

| Prop      | Type        | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| --------- | ----------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `url`     | `string`    | No       | The URL of the social login provider. If not provided, it's retrieved from navigation parameters.                                                                                                                                                                                                                                                                                                                                                             |
| `onResult` | `(userData: any) => void` | Yes      | Callback function called when user data is received from the WebView. Receives the user data as an argument.                                                                                                                                                                                                                                                                                                                                                              |

## Functionality

1. **WebView Loading:** The component renders a `WebView` that loads the specified `url`.

2. **Loading Indicator:** A loading indicator (`ActivityIndicator`) is displayed while the WebView is loading.

3. **JavaScript Enabled:** JavaScript is enabled in the WebView.

4. **User Agent:** A custom user agent is set for the WebView.

5. **Message Handling:** The component listens for messages from the WebView using the `onMessage` event.  It expects to receive user data as a JSON string in the message data.

6. **User Data Storage:** When user data is received, it is stored in AsyncStorage using the `LibSociallogin.setUser` method.

7. **User Data Retrieval:** The `LibSociallogin.getUser` method allows you to retrieve the stored user data.

8. **User Data Deletion:** The `LibSociallogin.delUser` method removes the stored user data.

## Important Considerations

*   **Social Login Provider:** You need to have a social login provider set up (e.g., using Firebase, Auth0, or a custom backend) and provide the correct URL.
*   **WebView Communication:** The social login provider's page needs to send a message to the WebView containing the user data as a JSON string.  This is typically done using `window.postMessage`.
*   **Data Format:** The component expects to receive user data as a JSON string.  Make sure your social login provider's page sends data in this format.
*   **AsyncStorage:** User data is stored in AsyncStorage.  Be aware of the limitations of AsyncStorage (e.g., data size limitations).
*   **Security:**  Be mindful of security best practices when implementing social login.  Avoid storing sensitive information directly in your client-side code.  Handle authentication and authorization securely on your backend.
*   **Error Handling:** The component doesn't include explicit error handling.  Consider adding error handling for cases where the WebView fails to load or the message data is invalid.
*   **Navigation:** You can pass the `url` as a navigation parameter, which is useful when navigating to the social login screen from another screen.
*   **User Agent:** The custom user agent is set in case the social login provider requires it. You might need to adjust it depending on the provider.
* **Loading State:** The `startInLoadingState` and `renderLoading` props of the `WebView` are used to display a loading indicator while the WebView is loading the social login page. This improves the user experience.