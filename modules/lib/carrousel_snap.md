# LibCarrousel_snap

This component creates a snapping carousel in React Native. It supports features like auto-cycling, looping, custom item rendering, and alignment options.

## Installation

This component relies on `esoftplay/esp` which is assumed to be available.  Ensure you have this library set up correctly in your project. Also, ensure you have `react-native` installed.

## Usage

```javascript
import { LibCarrousel_snap } from 'esoftplay/cache/lib/carrousel_snap/import';

// Example usage:
<LibCarrousel_snap
  data={myData}
  maxWidth={screenWidth} // Required: Width of the carousel container
  itemWidth={200}      // Required: Width of each item
  itemMarginHorizontal={10} // Margin between items
  autoCycle={true}       // Optional: Enables auto-cycling
  loop={true}           // Optional: Enables looping
  autoCycleDelay={3000}  // Optional: Auto-cycle delay in milliseconds
  align="center"       // Optional: Alignment of items ('center' or 'left')
  onChangePage={(page) => console.log("Current Page:", page)} // Optional: Callback for page changes
  initialPage={0}      // Optional: Initial page index
  renderItem={(item, key, itemWidth) => ( // Required: Function to render each item
    <View key={key} style={{ width: itemWidth, height: 150, backgroundColor: 'lightblue', justifyContent: 'center', alignItems: 'center' }}>
      <Text>{item.title}</Text>
    </View>
  )}
/>
```

## Props

| Prop                  | Type                 | Required | Default | Description                                                                                                                                                                |
| --------------------- | -------------------- | -------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `data`                | `any[]`              | Yes      |         | The data array to be displayed in the carousel.                                                                                                                            |
| `maxWidth`            | `number`             | Yes      |         | The maximum width of the carousel container.                                                                                                                            |
| `itemWidth`           | `number`             | Yes      |         | The width of each item in the carousel.                                                                                                                                    |
| `itemMarginHorizontal` | `number`             | Yes      | `0`      | Horizontal margin between items.                                                                                                                                          |
| `autoCycle`           | `boolean`            | No       | `false`  | Whether to automatically cycle through the items.                                                                                                                         |
| `loop`                | `boolean`            | No       | `false`  | Whether to loop the carousel (infinite scrolling).                                                                                                                             |
| `autoCycleDelay`      | `number`             | No       | `4000`   | Delay between auto-cycle transitions in milliseconds.                                                                                                                        |
| `align`               | `'center' \| 'left'` | No       | `center` | Alignment of the items within the carousel.                                                                                                                               |
| `onChangePage`        | `(page: number) => void` | No       |         | Callback function that is called when the current page changes. The `page` argument is the index of the current page.                                                  |
| `initialPage`         | `number`             | No       | `0`      | The initial page index to display.                                                                                                                                          |
| `renderItem`          | `(item: any, key: number \| string, itemWidth: number) => any` | Yes      |         | Function that renders each item in the carousel. Receives the item data, a unique key (number or string), and the item width as arguments. Should return a React element. |
| `style`               | `StyleProp<ViewStyle>` | No       |         | Style for the containing View.                                                                                                                                            |
| `...ScrollView props` |                      | No       |         | All other props that are valid for the React Native `ScrollView` component can be passed through.                                                                         |

## `renderItem` Function

The `renderItem` prop is a function that is responsible for rendering each item in the carousel.  It receives three arguments:

* `item`: The data for the current item.
* `key`: A unique key for the item.  This is crucial for React to efficiently update the list.  It can be a number or a string.
* `itemWidth`: The width of the item.

This function should return a React element that represents the item.

## Example with Data

```javascript
const myData = [
  { title: "Slide 1" },
  { title: "Slide 2" },
  { title: "Slide 3" },
  { title: "Slide 4" },
];

<LibCarrousel_snap
  data={myData}
  maxWidth={screenWidth}
  itemWidth={200}
  renderItem={(item, key, itemWidth) => (
    <View key={key} style={{ width: itemWidth, height: 150, backgroundColor: 'lightblue', justifyContent: 'center', alignItems: 'center' }}>
      <Text>{item.title}</Text>
    </View>
  )}
/>
```

## Important Considerations

* **`esoftplay/esp` Dependency:** This component relies on the `esoftplay/esp` library.  Make sure you have it properly installed and configured.  The component uses `esp.mod("lib/focus")` which is not standard React Native. You'll need to understand how `esp` works to ensure this part functions correctly.
* **`maxWidth` and `itemWidth`:** These props are essential for the correct layout of the carousel.  `maxWidth` should be the width of the carousel container, and `itemWidth` should be the width of each individual item.  Make sure these values are accurate.
* **Looping and Auto-cycling:** The `loop` and `autoCycle` props can be combined to create a carousel that automatically cycles through the items and loops infinitely.
* **`onChangePage` Callback:** The `onChangePage` callback can be used to track the current page index.  This can be useful for updating other parts of your application based on the carousel's current page.
* **`ScrollView` Props:**  You can pass any other valid `ScrollView` props to the `LibCarrousel_snap` component, such as `contentContainerStyle`, `onScroll`, etc.
* **Prefix and Suffix Rendering:** The component uses `prefix` and `suffix` functions to render items before the start and after the end of the data array when `loop` is enabled. This creates the seamless looping effect. The logic within these functions handles the edge cases for data arrays of length 1, 2, and greater than 2.
* **`LibFocus`:** The `LibFocus` component is used to pause the auto-cycle when the carousel loses focus (e.g., when the user interacts with another element).  This is handled by the `onFocus` and `onBlur` events.  Again, the interaction with `esp` is crucial here.
