# LibScrollpicker

This component implements a scrollable picker, allowing users to select a value from a list by scrolling. It provides a highlighted area to indicate the selected item and offers customization options for styling and rendering items.

## Installation

This component does not have any external dependencies beyond React Native itself.

## Usage

```javascript
import { LibScrollpicker } from 'esoftplay/cache/lib/scrollpicker/import';

<LibScrollpicker
  dataSource={['Item 1', 'Item 2', 'Item 3', 'Item 4', 'Item 5']}
  itemHeight={40}
  wrapperHeight={200}
  selectedIndex={0}
  highlightColor="blue"
  wrapperColor="#eee"
  renderItem={(item, index, isSelected) => (
    <Text style={{ color: isSelected ? 'red' : 'black' }}>{item}</Text>
  )}
  onValueChange={(value, index) => console.log("Selected value:", value, "at index:", index)}
/>
```

## Props

| Prop             | Type        | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| ---------------- | ----------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `itemHeight`     | `number`    | Yes      | The height of each item in the picker.                                                                                                                                                                                                                                                                                                                                                                     |
| `wrapperHeight`  | `number`    | Yes      | The height of the scroll picker's visible area.  Should be a multiple of `itemHeight`.                                                                                                                                                                                                                                                                                                                                                             |
| `selectedIndex`  | `number`    | Yes      | The initial index of the selected item.                                                                                                                                                                                                                                                                                                                                                                  |
| `style`          | `ViewStyle` | No       | Style for the scroll picker's container.  Can be used to set the width of the picker.                                                                                                                                                                                                                                                                                                                                                            |
| `highlightColor` | `string`    | No       | The color of the highlight border around the selected item.                                                                                                                                                                                                                                                                                                                                                                                     |
| `wrapperColor`   | `string`    | No       | The background color of the scroll picker's visible area.                                                                                                                                                                                                                                                                                                                                                                                    |
| `renderItem`     | `(item: any, index: number, isSelected: boolean) => any` | No       | A function to render each item in the picker. Receives the item data, index, and a boolean indicating whether the item is selected.  If not provided, a default `Text` component is used.                                                                        to:
| `dataSource`     | `any[]`     | Yes      | The data source for the picker. An array of values to be displayed.                                                                                                                                                                                                                                                                                                                                                                                    |
| `onValueChange`  | `(value: any, index: number) => void` | Yes      | Callback function called when the selected value changes. Receives the selected value and its index.                                                                                                                                                                                                                                                                                                                                                              |

## Functionality

1. **Scrollable List:** The component uses a `ScrollView` to create a scrollable list of items.

2. **Highlighted Selection:** A highlighted area visually indicates the currently selected item.

3. **Item Rendering:** The `renderItem` prop allows you to customize how each item is displayed. If not provided, a default `Text` component is used.

4. **Value Change Callback:** The `onValueChange` callback is called whenever the selected value changes due to scrolling.

5. **Scrolling Behavior:** The component handles scrolling events to update the selected index and call the `onValueChange` callback. It also includes logic to "snap" the scroll view to the nearest item when scrolling ends.

## Important Considerations

*   **Data Source:** The `dataSource` prop must be an array of values.
*   **Item Height and Wrapper Height:** The `itemHeight` and `wrapperHeight` props are crucial for the component to function correctly. `wrapperHeight` should be a multiple of `itemHeight`.
*   **`renderItem` Customization:** The `renderItem` prop provides a powerful way to customize the appearance of the items in the picker.
*   **Scrolling Events:** The component handles `onMomentumScrollBegin`, `onMomentumScrollEnd`, `onScrollBeginDrag`, and `onScrollEndDrag` events to manage the scrolling behavior and update the selected index.
*   **`_scrollFix` Function:** This function is responsible for "snapping" the scroll view to the nearest item. It also handles the logic for calling the `onValueChange` callback.
*   **Platform Specifics:** The `_scrollFix` function includes platform-specific logic (for iOS) to handle scrolling behavior.
*   **Accessibility:** Consider accessibility.  Use appropriate ARIA attributes to label the scroll picker and provide feedback to users with visual impairments.  Make sure the selected item is clearly indicated.
* **Initial Scroll:** The `componentDidMount` lifecycle method is used to scroll to the initial selected index.  The `setTimeout` with a 0ms delay is used to ensure that the component has rendered before attempting to scroll.
* **Unmounting:** The `componentWillUnmount` lifecycle method is used to clear any pending timers, preventing memory leaks.
* **getSelected()**: This method allows you to retrieve the currently selected value.
