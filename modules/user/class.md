# UserClass

This module provides utilities for managing user data, authentication, and push tokens in an Esoftplay React Native application.  It uses `AsyncStorage` for persistence and `useGlobalState` for in-memory state management.

## Installation

This module relies on several libraries. Ensure they are installed:

```bash
npm install @react-native-async-storage/async-storage esoftplay expo-constants expo-notifications react-fast-compare
# or
yarn add @react-native-async-storage/async-storage esoftplay expo-constants expo-notifications react-fast-compare
```

## API

### `state(): useGlobalReturn<any>`

Returns the global state object for user data.

### `create(user: any): Promise<void>`

Creates or updates the user data.  Triggers push token update if user data has changed and notifications are enabled.

### `load(callback?: (user?: any | null) => void): Promise<any>`

Loads user data from `AsyncStorage`.  Prioritizes in-memory state, falling back to `AsyncStorage`.

### `isLogin(callback: (user?: any | null) => void): Promise<any>`

Checks if a user is logged in (i.e., user data exists).

### `delete(): Promise<void>`

Deletes user data, resets notification badge count, and removes associated data. Triggers push token update if notifications are enabled.

### `pushToken(): Promise<any>`

Registers or updates the push token with the server.

## Functionality

1.  **State Management:** Uses `useGlobalState` to manage user data in memory and persist it using `AsyncStorage`.

2.  **User Creation/Update:** The `create` function updates the user data in both the global state and `AsyncStorage`.  It compares the old and new user data using `react-fast-compare` and triggers a push token update if the data has changed and notifications are enabled.

3.  **User Loading:** The `load` function retrieves user data, prioritizing the in-memory state (`state.get()`) and falling back to `AsyncStorage` if the in-memory state is empty.

4.  **Login Check:** The `isLogin` function checks if user data exists (either in memory or in `AsyncStorage`).

5.  **User Deletion:** The `delete` function clears user data from both memory and `AsyncStorage`, resets the notification badge count, deletes associated data, and triggers a push token update.

6.  **Push Token Management:** The `pushToken` function handles push token registration and updates:
    *   Requests notification permissions.
    *   Constructs a payload with user information, device info, and a secret key.
    *   Makes an API call to register/update the push token.
    *   Stores the push ID and token in `AsyncStorage`.
    *   Handles Expo app ownership checks.

## Important Considerations

*   **`AsyncStorage`:** Uses `AsyncStorage` for persistent storage of user data.
*   **Global State:** Uses `useGlobalState` for in-memory state management.
*   **Push Tokens:** Handles push token registration and updates.
*   **Security:** Uses a secret key for API calls.  Make sure the `config.salt` is securely managed.
*   **Error Handling:**  Error handling is present in the `pushToken` function but could be more robust.
*   **Dependencies:** Relies on several libraries, including `react-fast-compare`, `expo-constants`, `expo-notifications`, and `moment`.
*   **Configuration:** Reads configuration values from `esp.config()`.
*   **`react-fast-compare`:** Used for efficient comparison of user data.
*   **Expo Ownership Check:**  The `pushToken` function includes a check for Expo app ownership.  This might be relevant for development or certain deployment scenarios.
*   **Data Synchronization:** The module synchronizes user data between in-memory state and `AsyncStorage`.
*   **`push_id` Storage:** The `push_id` is stored in `AsyncStorage` and used in the push token registration payload.
*   **`token` Storage:** The push token itself is also stored in `AsyncStorage`.
*   **`user_notification` Removal:**  The `delete` function removes the `user_notification` entry from `AsyncStorage`.  This suggests a connection to notification-related data.
*   **`user/data` Deletion:** The `delete` function calls `esp.mod("user/data").deleteAll()`.  This indicates a dependency on another module (`user/data`) that manages user-related data, which is also cleared upon user deletion.
* **`moment` Library:** The `moment` library is used to generate a timestamp for the `secretkey`. Ensure it's correctly configured with the appropriate locale.
* **`lib/crypt` Module:** The `LibCrypt` module is used to encode the `secretkey`. Make sure this module is implemented and available.
* **`lib/notification` Module:** The `lib/notification` module is used to request notification permissions.
* **`lib/curl` Module:** The `LibCurl` module is used to make the API call for push token registration.
* **Error Handling:**  While there is some error handling in the `pushToken` function, it could be improved to handle various error scenarios (e.g., network errors, API errors).
* **Security:**  The `secretkey` generation uses a timestamp and a salt.  While this provides some level of security, consider more robust methods for securing API calls, especially in production environments.
* **Global State Management:**  The use of `useGlobalState` provides a simple way to manage user data.  However, for larger applications, consider a more structured state management solution like Redux or Context API.
* **Asynchronous Operations:**  The module uses Promises to handle asynchronous operations. This is a good practice for managing asynchronous code.
* **Data Consistency:**  The module attempts to maintain data consistency between the in-memory state and `AsyncStorage`.  However, it's important to be aware of potential issues related to asynchronous operations and data synchronization.
