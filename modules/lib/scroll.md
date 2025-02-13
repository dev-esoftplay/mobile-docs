# LibScroll

This component provides a scrollable view using React Native's `ScrollView`. It offers a simple way to wrap content within a scrollable area and provides some additional features like pull-to-refresh. It's designed for situations where you have a relatively small amount of content to scroll, as `ScrollView` renders all its children at once.  For large lists, consider using `LibList` instead.

## Installation

This component does not have any external dependencies beyond React Native itself.

## Usage

```javascript
import { LibScroll } from 'esoftplay/cache/lib/scroll/import'

<LibScroll
  onRefresh={() => { /* Optional refresh action */ }}
  refreshing={false} // Optional: Set to true while refreshing
  style={{flex: 1}} // Optional: Style for the ScrollView
>
  {/* Your scrollable content */}
  <View style={{ height: 200, backgroundColor: 'red' }} />
  <View style={{ height: 300, backgroundColor: 'blue' }} />
  <View style={{ height: 150, backgroundColor: 'green' }} />
</LibScroll>
```

## Props

| Prop                     | Type                                  | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| ------------------------ | ------------------------------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `bounces`                | `boolean`                             | No       | Whether the scroll view should bounce past the edges of the content.                                                                                                                                                                                                                                                                                                                                                            |
| `style`                  | `ViewStyle`                            | No       | Style for the ScrollView.                                                                                                                                                                                                                                                                                                                                                                                     |
| `ItemSeparatorComponent` | `any`                                 | No       | Component to render between items (if you're mapping an array of items).  If you are not mapping an array, this is not needed.                                                                                                                                                                                                                                                                                                                                                                          |
| `onScroll`               | `(e: any) => void`                    | No       | Callback function called when the scroll view is scrolled.                                                                                                                                                                                                                                                                                                                                                           |
| `scrollEventThrottle`    | `number`                              | No       | Number of milliseconds to throttle the `onScroll` event.                                                                                                                                                                                                                                                                                                                                                         |
| `keyboardShouldPersistTaps` | `boolean \| "always" \| "never" \| "handled"` | No       | Controls when the keyboard should be dismissed when tapping outside of text inputs.                                                                                                                                                                                                                                                                                                                                                            |
| `children`               | `any[]`                               | Yes      | The content to be rendered within the scroll view.                                                                                                                                                                                                                                                                                                                                                                              |
| `stickyHeaderIndices`    | `number[]`                            | No       | Array of indices for sticky headers within the scroll view.                                                                                                                                                                                                                                                                                                                                                                       |
| `pagingEnabled`          | `boolean`                             | No       | When `true`, the scroll view stops on multiples of the scroll view's visible bounds. Useful for paging through content.                                                                                                                                                                                                                                                                                                    |
| `horizontal`             | `boolean`                             | No       | Whether the scroll view should be displayed horizontally.                                                                                                                                                                                                                                                                                                                                                                |
| `initialNumToRender`     | `number`                              | No       | Number of items to render initially (if you are mapping an array of items).  This is not relevant if you are not mapping an array.                                                                                                                                                                                                                                                                                                                                                                            |
| `initialScrollIndex`     | `number`                              | No       | Index to scroll to initially (if you are mapping an array of items). This is not relevant if you are not mapping an array.                                                                                                                                                                                                                                                                                                                                                                              |
| `keyExtractor`           | `(item: any, index: number) => string` | No       | Function to extract a unique key for each item (if you are mapping an array of items).  This is not needed if you are not mapping an array.                                                                                                                                                                                                                                                                                                                                                                          |
| `numColumns`             | `number`                              | No       | Number of columns (if you are rendering items in a grid-like layout).  This is not relevant if you are not mapping an array.                                                                                                                                                                                                                                                                                                                                                                                     |
| `onRefresh`              | `(() => void) \| null`                 | No       | Callback function called when the user pulls down to refresh the content.                                                                                                                                                                                                                                                                                                                                                               |
| `refreshing`             | `boolean \| null`                      | No       | Whether the scroll view is currently refreshing.  You should set this to `true` while the refresh operation is in progress.                                                                                                                                                                                                                                                                                                                                                                                        |

## Functionality

1. **`ScrollView` Wrapper:** This component is a simple wrapper around React Native's `ScrollView`.

2. **Children Rendering:** It renders its `children` within the `ScrollView`.

3. **Pull-to-Refresh:** It supports pull-to-refresh functionality using the `RefreshControl` component.

4. **Scrolling Events:** It passes through scroll events via the `onScroll` prop.

5. **Sticky Headers:** It supports sticky headers using the `stickyHeaderIndices` prop.

## Important Considerations

*   **Large Lists:** `ScrollView` renders all its children at once, which can be inefficient for large lists.  For large datasets, use `LibList` which uses `FlatList` under the hood for better performance.
*   **Content Height:**  For `ScrollView` to work correctly, the content inside it must have a defined height (or width if `horizontal` is `true`).  If the content's height is dynamic and unknown, you might need to use a different approach (like measuring the content's height or using a `FlatList`).
*   **`onRefresh` and `refreshing`:**  When using the pull-to-refresh functionality, you need to manage the `refreshing` prop yourself.  Set it to `true` while the refresh operation is in progress, and set it back to `false` when the refresh is complete.
*   **Accessibility:** Consider accessibility.  Use appropriate ARIA attributes to label the scroll view and provide feedback to users with visual impairments.  For example, you could use `aria-live` to announce changes in the scroll view's content.
*   **Performance:**  For better performance, especially with many children, consider using `scrollEventThrottle` to limit the number of `onScroll` events.
*   **`ItemSeparatorComponent`, `initialNumToRender`, `initialScrollIndex`, `keyExtractor`, `numColumns`:**  These props are intended for use when rendering lists of items (e.g., mapping an array of data).  If you are not rendering a list of items, these props are not relevant.
* **`idxNumber` and `scrollToIndex`:** These are used for programmatically scrolling to a specific item when you are mapping an array of children.  The `idxNumber` array stores the calculated positions (x or y) of each item within the `ScrollView`.  The `scrollToIndex` function then uses these stored positions to scroll to the correct location.  Again, this is only relevant if you are mapping an array of items and need programmatic scrolling.  If you are not mapping an array, you can ignore these.
