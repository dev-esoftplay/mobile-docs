# LibNet_status

This component displays a network status banner at the top of the screen, indicating whether the device is online or offline. It uses `@react-native-community/netinfo` to monitor network connectivity changes.

## Installation

This component relies on `@react-native-community/netinfo` and `esoftplay`. Ensure you have them installed:

```bash
npm install @react-native-community/netinfo esoftplay
# or
yarn add @react-native-community/netinfo esoftplay
```

## Usage

```javascript
import { LibNet_status } from 'esoftplay/cache/lib/net_status/import';

// In your app's main component or a high-level component:
<LibNet_status />

//or calling the global state
const [{ isOnline, isInternetReachable }] = LibNet_status.state().useState()


```

## Props

This component accepts an optional prop:

| Prop       | Type      | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| ---------- | --------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `isOnline` | `boolean` | No       |  Used to set the status manually. This prop is intended for specific use cases where you need to override the automatic network status detection.  If not provided, the component will automatically determine the network status.                                                                                                                                                                                           |

## API

### `state()`

```typescript
static state(): useGlobalReturn<any>
```

Returns the global state object for the network status.  This allows other parts of your application to access the current online/offline state.

### `setOnline(isOnline: boolean, isInternetReachable: boolean)`

```typescript
static setOnline(isOnline: boolean, isInternetReachable: boolean): void
```

Sets the global network status.  This is used internally by the component to update the state when the network connectivity changes.  You might use it directly if you need to manually control the network status (e.g., for testing or specific scenarios).  The `isInternetReachable` parameter indicates whether the device is connected to the internet, even if it's connected to a network.

## Functionality

1. **Network Monitoring:** The component uses `@react-native-community/netinfo` to listen for network connectivity changes.

2. **Global State:** The network status (online/offline) and internet reachability are stored in a global state object (from `esoftplay`).

3. **Status Banner:** The component renders a banner at the top of the screen. The banner's background color and text indicate the current network status (green for online, red for offline).

4. **Animation:** The banner animates in and out smoothly when the network status changes.

5. **Language:** The "Online" and "Offline" text are localized using `esp.lang("lib/net_status", "online")` and `esp.lang("lib/net_status", "offline")`.

## Important Considerations

* **`@react-native-community/netinfo` Dependency:** Ensure that `@react-native-community/netinfo` is correctly installed and linked.
* **`esoftplay` Dependency:** This component relies on `esoftplay` for global state management and localization.
* **Global State:** The network status is stored in a global state object, making it accessible to other components in your application.
* **Animation:** The banner uses an `Animated.View` to provide a smooth transition when the network status changes.
* **Localization:** The text displayed in the banner is localized using `esp.lang`.  Make sure you have the appropriate language files set up in your `esoftplay` configuration.
* **Manual Override:** The `isOnline` prop can be used to manually override the detected network status, which can be useful for testing or specific use cases.
* **Internet Reachability:** The component distinguishes between being connected to a network and being able to reach the internet.  It uses the `isInternetReachable` property from `@react-native-community/netinfo` to make this distinction.  A device might be connected to a local network but not have internet access.
* **Accessibility:**  Consider accessibility.  Use appropriate ARIA attributes to indicate the network status to users with visual impairments.  For example, you could add an `aria-live` region that updates when the network status changes.  Ensure sufficient color contrast between the text and the background of the banner.
