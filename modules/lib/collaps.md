# LibCollaps

This component creates a collapsible section in React Native using `react-native-reanimated` for smooth animations. It allows you to expand and collapse content with a custom header.

## Installation

This component uses `react-native-reanimated` and `esoftplay/state`. Ensure you have these libraries installed in your project.

```bash
npm install react-native-reanimated
# or
yarn add react-native-reanimated

# For esoftplay/state, follow the library's specific installation instructions.
```

## Usage

```javascript
import { LibCollaps } from 'esoftplay/cache/lib/collaps/import';

function MyComponent() {
  const [isExpanded, setIsExpanded] = useState(false); // Or use your preferred state management

  return (
    <LibCollaps
      show={isExpanded} // Control the initial expansion state
      header={(isShow) => ( // Render the header
        <View style={{ backgroundColor: 'lightblue', padding: 10 }}>
          <Text>{isShow ? "Collapse" : "Expand"}</Text>
        </View>
      )}
      onToggle={(expanded) => setIsExpanded(expanded)} // Callback for toggle events
      style={{ borderWidth: 1, borderColor: 'gray', marginBottom: 10 }} // Style for the header/pressable
    >
      {/* Content to be collapsed/expanded */}
      <View style={{ padding: 10, backgroundColor: 'white' }}>
        <Text>This is the collapsible content.</Text>
      </View>
    </LibCollaps>
  );
}
```

## Props

| Prop        | Type                  | Required | Description                                                                                                                                                                                                                                                                                |
| ----------- | --------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `show`      | `boolean`             | No       | Controls the initial expansion state. If not provided, the component starts in a collapsed state.                                                                                                                                                                                          |
| `header`    | `(isShow: boolean) => any` | Yes      | A function that renders the header of the collapsible section.  Receives a boolean `isShow` indicating the current expansion state. Should return a React element.                                                                                                                             |
| `children`  | `any`                 | Yes      | The content to be displayed when the section is expanded.                                                                                                                                                                                                                                    |
| `onToggle`  | `(expanded: boolean) => void` | No       | A callback function that is called when the collapsible section is expanded or collapsed. Receives a boolean `expanded` argument indicating the new expansion state.                                                                                                                      |
| `style`     | `ViewStyle`           | No       | Style for the header/pressable View.                                                                                                                                                                                                                                                        |

## `header` Function

The `header` prop is a function that renders the header of the collapsible section. It receives one argument:

* `isShow`: A boolean value indicating whether the content is currently expanded (`true`) or collapsed (`false`).

This function should return a React element that represents the header.  This allows you to customize the header's appearance and content based on the expansion state.

## `onToggle` Callback

The `onToggle` prop is an optional callback function that is called whenever the collapsible section is expanded or collapsed.  It receives one argument:

* `expanded`: A boolean value indicating the new expansion state (`true` for expanded, `false` for collapsed).

This callback can be used to update other parts of your application based on the collapsible section's state.

## Styling

The `style` prop allows you to apply styles to the header/pressable View.  You can use this to control the appearance of the header, such as background color, padding, border, etc. The content below the header can be styled directly by wrapping it in a View.

## Animation

The component uses `react-native-reanimated` for smooth animations.  The opacity and translateY are animated when the section is expanded or collapsed. The animation duration is currently set to 300 milliseconds.

## `esoftplay/state`

The component utilizes `esoftplay/state`'s `useSafeState` hook.  Ensure that this library is correctly set up in your project.  If you are not using `esoftplay` then you can replace `useSafeState` with `useState` from react.

## Example with State

```javascript
import React, { useState } from 'react'; // Import useState
import { LibCollaps } from 'esoftplay/cache/lib/collaps/import';

function MyComponent() {
  const [isExpanded, setIsExpanded] = useState(false);

  return (
    <LibCollaps
      show={isExpanded}
      header={(isShow) => (
        <View style={{ backgroundColor: 'lightblue', padding: 10 }}>
          <Text>{isShow ? "Collapse" : "Expand"}</Text>
        </View>
      )}
      onToggle={(expanded) => setIsExpanded(expanded)}
      style={{ borderWidth: 1, borderColor: 'gray', marginBottom: 10 }}
    >
      <View style={{ padding: 10, backgroundColor: 'white' }}>
        <Text>This is the collapsible content.</Text>
      </View>
    </LibCollaps>
  );
}
```
