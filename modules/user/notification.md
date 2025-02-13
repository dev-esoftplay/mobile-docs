```markdown
# UserNotification

This component displays a list of user notifications. It fetches notification data, renders each notification item, and handles navigation when a notification is pressed.

## Installation

This component relies on several `esoftplay` modules. Ensure they are installed:

```bash
npm install esoftplay
# or
yarn add esoftplay
```

## Usage

```javascript
import { UserNotification } from 'esoftplay/cache/user/notification/import';

// In your navigation or screen component:
<UserNotification navigation={navigation} data={[]} /> // Pass navigation prop
```

## API

### `UserNotification` (Component)

Renders the list of notifications.

### `static state(): useGlobalReturn<any>`

Returns the global state object for notification data.

## Props

| Prop         | Type    | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| ------------ | ------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `navigation` | `any`   | Yes      | The navigation object (usually passed from React Navigation).                                                                                                                                                                                                                                                                                                                                                                                     |
| `data`       | `any[]` | Yes      | Initial notification data. Although it's marked as required, the component fetches data itself, so an empty array `[]` is usually passed here.  The component uses the global state for the actual data.  This prop is likely included for initial rendering or potential future use cases.|

## State

The component uses a global state object (`state`) to store and manage notification data.

## Functionality

1.  **Notification Data:** Fetches and manages notification data using a global state object (`state`).  The `state` object is initialized with an empty array for `data`, empty arrays for `urls`, and a count of unread notifications (`unread`).  The `persistKey` is used to persist the state across app restarts.  The `isUserData` flag suggests that this data is considered user-specific.  The `loadOnInit` flag indicates that the state should be loaded from persistence when the component initializes.

2.  **Rendering:** Renders a list of notifications using `LibList`.

3.  **Notification Item:** Each notification is rendered using the `UserNotification_item` component.

4.  **Navigation:** Handles navigation when a notification item is pressed, likely opening a detailed view of the notification using `LibNotification.openNotif`.

5.  **Refresh:** Allows refreshing the notification list using `LibNotification.loadData`.

6.  **Header:** Displays a header with a back button and the title "Notifikasi".

7.  **Styling:** Uses `LibStyle` for styling.

8.  **Status Bar:** Uses `LibStatusbar` to manage the status bar style.

9. **Global State Connection:** The component connects to the global state using `state.connect`. This allows the component to re-render whenever the notification data in the global state changes. The `render` prop of the `state.connect` component receives the current state as an argument and uses it to render the list of notifications.

## Important Considerations

*   **`esoftplay` Dependencies:** Relies on several `esoftplay` modules for styling, icons, lists, notifications, and status bar management.
*   **Global State:** Uses a global state object to manage notification data.
*   **Navigation:** Uses React Navigation for navigation.
*   **Notification Handling:** Uses `LibNotification` for fetching data and opening notifications.
*   **Styling:** Uses `LibStyle` for styling.
*   **Data Fetching:** The `onRefresh` prop of the `LibList` component triggers a refresh of the notification data.
*   **Notification Item Component:** The `UserNotification_item` component is responsible for rendering the individual notification items.
*   **Header Styling:** The header includes a back button and a title. The styling of the header can be customized using `LibStyle`.
*   **Status Bar Styling:** The `LibStatusbar` component is used to set the style of the status bar.
* **Component Structure:** The component uses a functional component structure with hooks.
* **Global State Management:**  The use of `useGlobalState` from `esoftplay` is a key aspect of this component.  It allows the notification data to be shared and updated across different parts of the application.  The `persistKey`, `isUserData`, and `loadOnInit` options provide persistence and control over how the state is loaded and saved.
* **`LibList` Component:** The `LibList` component is used to efficiently render the list of notifications.  It likely handles virtualization and other performance optimizations for long lists.
* **`TouchableOpacity` for Notifications:** Wrapping the `UserNotification_item` component in a `TouchableOpacity` makes each notification item pressable, triggering the `LibNotification.openNotif` function.
* **Navigation with `goBack`:** The back button in the header uses `this.props.navigation.goBack()` to navigate back.
* **Styling with `LibStyle`:** The component consistently uses `LibStyle` for styling, ensuring a consistent look and feel throughout the application.  It accesses various style properties like `colorPrimary`, `colorAccent`, and `STATUSBAR_HEIGHT`.
* **Conditional Rendering (Implicit):** The `state.connect` component implicitly handles conditional rendering.  If the `data` array is empty, the `LibList` will likely render an empty state message or nothing at all.
* **Data Flow:** The data flows from the global state to the `LibList` component, which then renders the individual `UserNotification_item` components.  When a notification is pressed, `LibNotification.openNotif` is called, likely triggering further actions or navigation.
* **`LibNotification` Interaction:** The component interacts with the `LibNotification` module for data fetching (`loadData`) and handling notification clicks (`openNotif`).  This suggests that `LibNotification` handles the underlying logic for these operations.