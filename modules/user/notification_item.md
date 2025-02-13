# UserNotification_item

This component displays a single notification item within a list of notifications. It shows the notification title, message, and timestamp.

## Installation

This component relies on `esoftplay`. Ensure it's installed:

```bash
npm install esoftplay
# or
yarn add esoftplay
```

## Usage

```javascript
import { UserNotification_item } from 'esoftplay/cache/user/notification_item/import';

<UserNotification_item
  created="2024-07-27 10:00:00"
  id={1}
  message="Your order has been shipped!"
  notif_id={123}
  params=""
  return=""
  status={1} // 1: unread, 2: read
  title="Order Update"
  updated="2024-07-27 12:00:00"
  user_id={456}
/>
```

## Props

| Prop        | Type     | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| ----------- | -------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `created`   | `string` | Yes      | The creation timestamp of the notification.                                                                                                                                                                                                                                                                                                                                                                                     |
| `id`        | `number` | Yes      | The unique ID of the notification.                                                                                                                                                                                                                                                                                                                                                                                     |
| `message`   | `string` | Yes      | The message content of the notification.                                                                                                                                                                                                                                                                                                                                                                                     |
| `notif_id`  | `number` | Yes      | Another ID associated with the notification (likely notification service ID).                                                                                                                                                                                                                                                                                                                                                      |
| `params`    | `string` | Yes      | Additional parameters (likely JSON string).                                                                                                                                                                                                                                                                                                                                                                                     |
| `return`    | `string` | Yes      | Return value or data (likely JSON string).                                                                                                                                                                                                                                                                                                                                                                                     |
| `status`    | `number` | Yes      | The status of the notification (e.g., 1 for unread, 2 for read).                                                                                                                                                                                                                                                                                                                                                                                 |
| `title`     | `string` | Yes      | The title of the notification.                                                                                                                                                                                                                                                                                                                                                                                     |
| `updated`   | `string` | Yes      | The update timestamp of the notification.                                                                                                                                                                                                                                                                                                                                                                                     |
| `user_id`   | `number` | Yes      | The ID of the user who received the notification.                                                                                                                                                                                                                                                                                                                                                                                     |

## Functionality

1.  **Layout:** Uses a `View` with padding, a row layout (`flexDirection: "row"`), white background, margin, and elevation for styling the notification item.

2.  **Content:** Displays the notification title, message (truncated to two lines), and the time since the notification was updated.

3.  **Styling:** Uses `LibStyle` (presumably from `esoftplay`) for consistent styling.

4.  **Timestamp:** Uses `LibUtils.moment(props.updated).fromNow()` (likely from `esoftplay`) to format the timestamp in a user-friendly way (e.g., "2 hours ago").

5.  **Read Status:** The title color changes based on the `status` prop (gray for read, primary color for unread).

## Important Considerations

*   **`esoftplay` Dependencies:** Relies on `esoftplay`'s `LibStyle` and `LibUtils`.
*   **Styling:** The styling can be customized using `LibStyle`.
*   **Timestamp Formatting:** The `LibUtils.moment().fromNow()` function provides a relative timestamp.
*   **Read/Unread Status:** The title color indicates the read status of the notification.
*   **Props:** All props are required and provide the necessary information for displaying the notification item.
*   **Truncation:** The message is truncated to two lines using `ellipsizeMode="tail"` and `numberOfLines={2}`.
*   **Elevation:** The component uses `LibStyle.elevation(1.5)` to add a subtle shadow to the notification item.  This gives it a more visually appealing look and helps to separate the items in the list.
*   **Margins and Padding:** The component uses margins and padding to create spacing around the notification content.
*   **Width:** The component's width is set to `LibStyle.width`, likely to occupy the full width of the screen.
* **Accessibility:** Consider adding accessibility features to this component, such as an `accessible` prop and an `aria-label` for screen readers.  This will make the notification items more accessible to users with disabilities.
* **Component Reusability:** This component is designed to be reusable and can be used to display different notification items with varying content.  The use of props makes it easy to customize the content and appearance of each notification.