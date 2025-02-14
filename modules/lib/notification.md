# LibNotification

This module provides comprehensive functionality for managing push notifications and local notifications in an Esoftplay React Native application. It includes features for handling notification reception, fetching notification data, marking notifications as read, scheduling local notifications, and navigating based on notification actions.

## Installation

This module depends on several libraries. Ensure they're installed:

```bash
npm install expo-constants expo-notifications @react-native-async-storage/async-storage esoftplay
# or
yarn add expo-constants expo-notifications @react-native-async-storage/async-storage esoftplay
```

## API

### `state(): useGlobalReturn<any>`

Returns the global state object related to notifications (specifically, the last fetched URL).

### `loadData(isFirst?: boolean): void`

Fetches notification data from the server.

### `add(id: number, user_id: number, group_id: number, title: string, message: string, params: string, status: 0 | 1 | 2, created?: string, updated?: string): void`

Adds a new notification item to the local state.

### `drop(): void`

Resets the notification data in the global state.

### `markRead(id?: string | number, ..._ids: (string | number)[]): void`

Marks one or more notifications as read.

### `onAction(notification: any): void`

Handles notification actions (when a notification is tapped).

### `getData(x: any): any`

Extracts notification data from the notification object.

### `listen(dataRef: any): void`

Sets up notification listeners and requests notification permissions.

### `requestPermission(callback?: (token: any) => void): Promise<any>`

Requests notification permissions from the user.

### `cancelLocalNotification(notif_id: string): void`

Cancels a scheduled local notification.

### `setLocalNotification(action: "alert" | "default" | "update", content: notificationContent, trigger: notificationTrigger, callback?: (notif_id: string) => void): void`

Schedules a local notification.

### `openPushNotif(data: any): void`

Handles opening a push notification based on its data.

### `openNotif(data: any): void`

Handles opening a notification (push or local) based on its data.

## Functionality

1.  **Notification Handling:** Sets up a notification handler to customize how notifications are displayed.

2.  **Data Fetching:** The `loadData` function fetches notification data from the server, handling pagination and storing the data in the global state.

3.  **Adding Notifications:** The `add` function adds a new notification to the local state, useful for updating the notification list without a full refresh.

4.  **Clearing Notifications:** The `drop` function resets the notification data.

5.  **Marking as Read:** The `markRead` function updates the status of notifications to "read" and sends a request to the server to mark them as read.

6.  **Notification Actions:** The `onAction` function is called when a notification is tapped. It extracts the notification data and calls `openPushNotif` to handle the action.

7.  **Data Extraction:** The `getData` function extracts the relevant data from the notification object.

8.  **Notification Listening:** The `listen` function requests notification permissions, sets up notification channels (Android), and adds a listener for received notifications.

9.  **Permission Request:** The `requestPermission` function requests notification permissions and retrieves the Expo push token.

10. **Local Notifications:** The `cancelLocalNotification` and `setLocalNotification` functions allow scheduling and canceling local notifications.

11. **Opening Notifications:** The `openPushNotif` and `openNotif` functions handle opening notifications based on their type (alert or default) and navigate to the appropriate screen or display an alert.

## Important Considerations

*   **`expo-notifications`:** Heavily relies on `expo-notifications` for handling notifications.
*   **Global State:** Uses `useGlobalState` for managing notification data.
*   **API Calls:** Makes API calls to fetch notification data and mark notifications as read.
*   **Navigation:** Uses `LibNavigation` for navigating based on notification actions.
*   **Local Notifications:** Supports scheduling and canceling local notifications.
*   **Error Handling:** Basic error handling is present but could be more robust.
*   **Configuration:** Reads configuration values from `esp.config()`.
*   **Language Support:** Uses `esp.lang` for localized strings.
*   **Security:** Uses `LibCrypt` for encrypting API requests.
*   **Data Structure:** Assumes a specific structure for notification data.
*   **Notification Channels (Android):** Sets up notification channels for Android.
*   **Background Handling:** The `handleNotification` function is crucial for handling notifications when the app is in the background or closed.
* **Notification Payload Structure:** The code expects a specific structure for the notification payload, including `action`, `module`, `params`, `title`, and `message` within the `data` field.  This structure is used to determine how to handle the notification.
* **`lastUrlState`:** This global state variable is used for pagination of notifications. It stores the URL of the next page of notifications to be fetched.
* **`readAll` Function:** This function handles marking multiple notifications as read by sending a batch request to the server.  It splits the array of notification IDs into chunks to avoid exceeding URL length limits.
* **`splitArray` Function:** This utility function is used to split an array into chunks of a specified size, which is necessary for the `readAll` function.
* **`openPushNotif` and `openNotif` Functions:** These functions handle opening the notification based on its type (alert or default).  They use `LibNavigation` to navigate to the appropriate screen or display an alert.
* **`setLocalNotification` Function:** This function allows scheduling local notifications with custom content and triggers.
* **`listen` Function:** This function sets up the necessary listeners for incoming notifications and requests notification permissions.  It's crucial for the app to receive and handle notifications.
* **`requestPermission` Function:** This function requests notification permissions from the user and retrieves the Expo push token.  It's essential for sending push notifications to the device.
* **`Notifications.setNotificationHandler`:** This is the core function for handling incoming notifications.  It defines how the app should respond to notifications when they are received.
* **`Notifications.setNotificationChannelAsync`:** This function is used to create notification channels on Android.  Notification channels are required for Android 8.0 (API level 26) and higher.  They allow users to customize notification settings for different categories of notifications.
* **`Notifications.scheduleNotificationAsync`:** This function is used to schedule local notifications.
* **`Notifications.cancelScheduledNotificationAsync`:** This function is used to cancel a scheduled local notification.
* **Error Handling:**  While there is some error handling (e.g., in the `requestPermission` function), it could be made more robust to handle various error scenarios (e.g., network errors, API errors, permission errors).
* **Security:**  The use of `LibCrypt` for encrypting API requests is important for security.  However, ensure that the encryption mechanism is strong enough and that the secret key is properly managed.
* **Performance:**  Be mindful of performance when handling a large number of notifications.  The use of pagination and efficient list rendering is important.
* **Testing:**  Thoroughly test all notification-related functionality, including receiving notifications, handling notification actions, scheduling local notifications, and handling background notifications.