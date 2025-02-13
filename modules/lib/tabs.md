# LibTabs

This component implements a tabbed interface with swipe support. It allows you to display different views within tabs and provides options for lazy loading and controlling the number of preloaded tabs.

## Installation

This component relies on `esoftplay`. Ensure you have it installed:

```bash
npm install esoftplay
# or
yarn add esoftplay
```

## Usage

```javascript
import { LibTabs } from 'esoftplay/cache/lib/tabs/import';

// Array of tab view components
const TabViews = [
  <View style={{ flex: 1, backgroundColor: 'red' }}><Text>Tab 1 Content</Text></View>,
  <View style={{ flex: 1, backgroundColor: 'blue' }}><Text>Tab 2 Content</Text></View>,
  <View style={{ flex: 1, backgroundColor: 'green' }}><Text>Tab 3 Content</Text></View>
]

<LibTabs
  tabIndex={currentTabIndex} // Current active tab index
  onChangeTab={(index) => setCurrentTabIndex(index)} // Callback for tab changes
  tabViews={TabViews}
  tabProps={[{}, {}, {}]} // Optional props for each tab view
  swipeEnabled={true} // Enable swiping between tabs
  defaultIndex={0} // Default active tab index
  tabOffset={1} // Number of tabs to preload on each side of the active tab
  lazyTabOffset={true} // Only include tabs from tabOffset to tabOffset + 1
/>
```

## Props

| Prop             | Type        | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| ---------------- | ----------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `tabIndex`       | `number`    | Yes      | The current active tab index.  This prop should be controlled by a parent component.                                                                                                                                                                                                                                                                                                                                                              |
| `onChangeTab`    | `(index: number) => void` | No       | Callback function called when the active tab changes (either by swipe or programmatically). Receives the new tab index.                                                                                                                                                                                                                                                                                                                                                           |
| `defaultIndex`   | `number`    | No       | The default active tab index (if `tabIndex` is not initially provided).                                                                                                                                                                                                                                                                                                                                                              |
| `swipeEnabled`   | `boolean`    | No       | Whether swiping between tabs is enabled. Defaults to `true`.                                                                                                                                                                                                                                                                                                                                                            |
| `tabViews`       | `any[]`     | Yes      | An array of React components or elements to be displayed as tab views.                                                                                                                                                                                                                                                                                                                                                                       |
| `tabProps`       | `any[]`     | No       | An optional array of props to be passed to each corresponding tab view component. Should be the same length as `tabViews`.                                                                                                                                                                                                                                                                                                                                                         |
| `tabOffset`      | `number`    | No       | The number of tabs to preload on each side of the currently active tab.  Used for lazy loading. Defaults to `1`.                                                                                                                                                                                                                                                                                                                                                                        |
| `lazyTabOffset`  | `boolean`   | No       | If true, only tabs from tabOffset to tabOffset + 1 will be rendered. If false, all tabs will be rendered.                                                                                                                                                                                                                                                                                                                                                                        |

## Functionality

1. **Horizontal Scrolling:** The component uses a `ScrollView` with `horizontal` and `pagingEnabled` props to create the tabbed interface with swipe functionality.

2. **Tab Views:** The `tabViews` prop is an array of React components that represent the content of each tab.

3. **Lazy Loading:** The `tabOffset` prop allows you to control how many tabs are preloaded. This helps improve performance, especially when you have many tabs.  The component only renders the currently active tab and a few adjacent tabs.

4. **Tab Props:** The `tabProps` prop allows you to pass specific props to each tab view component.

5. **Tab Index Control:** The `tabIndex` prop is used to control the currently active tab.  This prop *must* be controlled by a parent component.

6. **Tab Change Callback:** The `onChangeTab` prop is a callback function that is called whenever the active tab changes.  This allows the parent component to update the `tabIndex` and perform other actions.

7. **Dynamic Tab Rendering:** The `allIds` array and the `includes` method are used to control which tabs are rendered. This allows for dynamic rendering and lazy loading of tabs.

## Important Considerations

*   **Controlled Component:** The `tabIndex` prop *must* be controlled by a parent component.  The `LibTabs` component itself does not manage the active tab index. It relies on the parent to provide the `tabIndex` and update it via the `onChangeTab` callback.
*   **Lazy Loading:** The `tabOffset` and `lazyTabOffset` props are important for optimizing performance when you have many tabs.  They control how many tabs are rendered at any given time.
*   **`tabViews` and `tabProps` Length:** The `tabViews` and `tabProps` arrays should have the same length.
*   **Styling:** You can style the tabs and their content using standard React Native styling techniques.
*   **Accessibility:** Consider accessibility. Use appropriate ARIA attributes to make the tabs accessible to users with visual impairments.
*   **Performance:** For a very large number of tabs, consider further optimizations, such as using a virtualized list for the tab bar itself (if you have a custom tab bar) or more aggressive lazy loading.
* **Component Lifecycle:** The `componentDidUpdate` method is used to update the displayed tabs when the `tabIndex` prop changes.  This is essential for the controlled component behavior.
* **`allIds` and `arrayOfLimit`:** These methods are used to implement the lazy loading logic.  `arrayOfLimit` calculates the range of tab indices that should be rendered based on the current `tabIndex` and `tabOffset`.  `allIds` keeps track of which tabs have been rendered so far.  This prevents unnecessary re-renders.
* **`scrollRef`:** The `scrollRef` is used to programmatically scroll to the correct tab when the `tabIndex` changes.  This ensures smooth transitions between tabs.