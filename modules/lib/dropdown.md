# LibDropdown

This component creates a customizable dropdown select for React Native. It supports custom item rendering, labels, styling, and positioning.

## Installation

This component relies on `esoftplay`. Ensure you have it installed in your project.

```bash
npm install esoftplay
# or
yarn add esoftplay
```

## Usage

```javascript
import { LibDropdown } from 'esoftplay/cache/lib/dropdown/import';

const options = [
  { id: 1, value: 'Option 1' },
  { id: 2, value: 'Option 2' },
  { id: 3, value: 'Option 3' },
];

<LibDropdown
  value={options[0]} // Initial selected value
  options={options}
  renderItem={(item, index) => ( // Render each option
    <Pressable onPress={() => { setCurrentValue(item); setIsOpen(false); }}>
      <Text style={{ padding: 10 }}>{item.value}</Text>
    </Pressable>
  )}
  label="Select an option" // Optional label
  style={{ width: 200 }} // Optional style for the input
  popupStyle={{  }} // Optional style for the popup
  maxPopupHeight={200} // Optional max height for the popup
/>
```

## Props

| Prop            | Type                 | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| --------------- | -------------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `value`         | `LibDropdownOption`  | Yes      | The currently selected option.                                                                                                                                                                                                                                                                                                                                                                                    |
| `options`       | `LibDropdownOption[]` | Yes      | An array of available options.                                                                                                                                                                                                                                                                                                                                                                                   |
| `renderItem`    | `(item: LibDropdownOption, index: number) => any` | Yes      | Function to render each option in the dropdown list.  Receives the item data and index as arguments. Should return a React element (typically a `Pressable` with a `Text` inside).                                                                                                                                                                                                |
| `label`         | `string`             | No       | Optional label to display as a placeholder in the input.                                                                                                                                                                                                                                                                                                                                                         |
| `fixOffsetTop`  | `boolean`            | No       | Optional. If true, it will subtract statusbar height from the top offset of the popup. Useful for fixing positioning issues on Android.                                                                                                                                                                                                                                                                                                                                                        |
| `style`         | `ViewProps`          | No       | Optional style for the input (the `Pressable` and `TextInput` combination).                                                                                                                                                                                                                                                                                                                                                                                                                   |
| `popupStyle`    | `ViewProps`          | No       | Optional style for the popup container.                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| `maxPopupHeight` | `number`             | No       | Optional maximum height for the popup. If the options exceed this height, the list will scroll. Defaults to 140.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |

## `LibDropdownOption` Interface

```typescript
interface LibDropdownOption {
  id: string | number,
  value: any
}
```

This interface defines the structure of the options.  Each option must have an `id` (string or number) and a `value` (can be any type).

## Functionality

1. **Controlled Component:** The component is controlled, with the `currentValue` state variable reflecting the selected option.

2. **Popup Toggle:** The `togglePopup` function opens and closes the dropdown list.

3. **Positioning:** The popup's position is calculated using `DropdownRef.current?.measure` to position it correctly relative to the input.  The `fixOffsetTop` prop is provided to handle status bar height on Android if needed.

4. **Modal:** The dropdown list is displayed in a `Modal` component, which is positioned absolutely.

5. **FlatList:** The list of options is rendered using a `FlatList` for efficient rendering of potentially long lists.

6. **Custom Rendering:** The `renderItem` prop allows complete customization of how each option is displayed.

7. **Accessibility:** Consider adding accessibility features to the dropdown, such as labels, ARIA attributes, and keyboard navigation support.

## Important Considerations

* **`esoftplay` Dependency:** This component depends on `esoftplay`.  Make sure it's installed and configured correctly.
* **Positioning:** The popup positioning logic relies on measuring the input's dimensions.  Make sure the input's layout is stable before the popup is shown.
* **Custom `renderItem`:** The `renderItem` prop is crucial for customizing the look and feel of the dropdown items.  Make sure to implement it appropriately.  A common pattern is to use a `Pressable` to make the items selectable and update the `currentValue` state.
* **Modal and Overlay:** The `Modal` provides a way to overlay the dropdown on top of other content.
* **Performance:** For very large lists of options, consider optimizing the `renderItem` function and potentially using virtualization techniques in the `FlatList`.
* **Platform Differences:** The `Platform.OS` check adjusts for potential platform differences in positioning, particularly related to the status bar on Android.
* **Styling:** The `style` and `popupStyle` props allow for flexible styling of the input and the dropdown popup, respectively.
