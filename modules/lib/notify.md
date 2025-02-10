```markdown
# LibNotify

This module provides a function `libnotify` to send push notifications using Expo's push notification service. It handles decoding the recipient token (if necessary) and making the API request.  It also stores failed notifications for later retry.

## Installation

This module relies on `esoftplay`. Ensure you have it installed:

```bash
npm install esoftplay
# or
yarn add esoftplay
```

## Usage

```javascript
import { LibNotify } from 'esoftplay/cache/lib/notify/import';

const notifications = [
  {
    to: 'ExponentPushToken[your_push_token]', // Or the decoded token
    sound: 'default',
    title: 'Hello!',
    body: 'This is a notification.',
    data: { someData: 'goes here' },
  },
  // ... more notifications
];

LibNotify({ notifications });
```

## API

### `LibNotify(res: LibNotifyProps)`

```typescript
LibNotify(res: LibNotifyProps): any
```

Sends one or more push notifications.  The `res` argument is an object with a `notifications` property, which is an array of `LibNotifyItem` objects.

## Interfaces

### `LibNotifyItem`

```typescript
interface LibNotifyItem {
  to: string,
  sound: string,
  title: string,
  body: string,
  data: any,
}
```

Represents a single push notification.

*   `to`: The recipient token (Expo push token or the decoded token).
*   `sound`: The sound to play when the notification is received.
*   `title`: The title of the notification.
*   `body`: The body text of the notification.
*   `data`: Any additional data to send with the notification.

### `LibNotifyProps`

```typescript
interface LibNotifyProps {
  notifications: LibNotifyItem[]
}
```

The props object passed to `libnotify`.

*   `notifications`: An array of `LibNotifyItem` objects.

## Functionality

1. **Token Decoding:** The `curl` function checks if the `to` token includes "ExponentPushToken". If not, it assumes the token is encoded and decodes it using `LibCrypt`.

2. **API Request:** The `curl` function makes a POST request to the Expo push notification API (`https://exp.host/--/api/v2/push/send`) with the notification data.

3. **Error Handling:** If the API request fails, the notification data is stored in a global state (using `useGlobalState` from `esoftplay`) for later retry.  This is a basic form of offline persistence.

4. **Batch Sending:** The `libnotify` function iterates over the `notifications` array and calls the `curl` function for each notification.

## Important Considerations

*   **Expo Push Tokens:** The `to` field should contain a valid Expo push token.
*   **Token Encoding:** If the token is encoded, ensure you are using the correct decoding mechanism (via `LibCrypt`).
*   **Error Handling:** The current error handling stores failed notifications in global state. You might want to implement a more robust retry mechanism or logging.
*   **`esoftplay` Dependency:** This module relies on `esoftplay` for global state management and cryptography.
*   **API Endpoint:** The module uses the Expo push notification API endpoint.
*   **Background Sending:**  For sending notifications when the app is in the background or closed, you'll need to configure a background task or use a push notification service that supports background delivery.
*   **Security:** Be mindful of security best practices when handling push tokens.  Avoid storing them directly in your client-side code if possible.  Consider using a secure backend service to manage push notifications.
*   **Retry Mechanism:**  The module currently stores failed notifications.  You'll need to implement a retry mechanism to resend those notifications later.
* **Global State:** Failed notifications are stored in global state. This might not be suitable for production applications. Consider using a more persistent storage solution.
* **Localization:**  You might want to localize the notification title and body.
* **Data Payload:** The `data` field allows you to send additional data with the notification. This data can be used to perform actions in your app when the notification is received.
* **Permissions:**  You'll need to request push notification permissions in your app before you can send push notifications.
* **Testing:**  Use Expo's push notification tool or a service like Pusher Beams to test your push notifications.