```markdown
# LibNumbermeter

This component provides a number input control with a meter-like visual representation. Users can select a number within a specified range by scrolling through a series of vertical bars.

## Installation

This component relies on `esoftplay`. Ensure you have it installed:

```bash
npm install esoftplay
# or
yarn add esoftplay
```

## Usage

```javascript
import { LibNumbermeter } from 'esoftplay/cache/lib/numbermeter/import';

<LibNumbermeter
  range={[0, 100]}
  onValueChange={(value) => console.log("Value changed:", value)}
  valueDisplayEdit={(value) => value + " units"} // Optional: Format displayed value
/>
```

## Props

| Prop             | Type        | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| ---------------- | ----------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `range`          | `[number, number]` | Yes      | An array containing the minimum and maximum values of the number meter.                                                                                                                                                                                                                                                                                                                                                             |
| `onValueChange`  | `(value: number) => void` | Yes      | Callback function called when the selected value changes. Receives the new value as a number.                                                                                                                                                                                                                                                                                                                                                              |
| `valueDisplayEdit` | `(value: string) => string` | No       | Optional callback function to format the displayed value in the text input. Receives the value as a string and should return the formatted string to display.                                                                                                                                                                                                                                                                                        |

## Functionality

1. **Value Interpolation:** The `interpolate` function maps the scroll position of the `FlatList` to a value within the specified range.

2. **Value Display:** A `TextInput` displays the currently selected value.  The `valueDisplayEdit` prop can be used to customize how the value is displayed (e.g., adding units).

3. **Meter Visualization:** A `FlatList` is used to create the meter visualization.  Each item in the list is a vertical bar. The height of the bars varies to provide a visual cue of the selected value.

4. **Scrolling and Value Update:** When the `FlatList` is scrolled, the `onScroll` event handler calculates the corresponding value using the `interpolate` function.  It then updates the `TextInput` display and calls the `onValueChange` callback with the new value.

5. **Gradient Overlay:** Gradient overlays are used on both sides of the meter to visually indicate the start and end of the range.

## Important Considerations

* **`esoftplay` Dependency:** This component relies on `esoftplay` for font, gradient, icon, and styling utilities. Ensure it is correctly installed.
* **Range:** The `range` prop must be a tuple of two numbers representing the minimum and maximum values.
* **`onValueChange` Callback:** The `onValueChange` callback is essential for getting the selected value.
* **`valueDisplayEdit` Callback:** The `valueDisplayEdit` callback is optional but can be useful for formatting the displayed value.
* **Performance:** The `FlatList` uses `scrollEventThrottle` to optimize performance. However, for very large ranges, you might need to further optimize the component.
* **Customization:** The appearance of the meter (bar height, colors, etc.) can be customized by modifying the styles.
* **Accessibility:** Consider accessibility.  Ensure that the control is usable with assistive technologies.  You might need to add ARIA attributes or use a different approach for users who cannot see the visual meter.  Provide an accessible way to set the value, such as direct text input or using increment/decrement buttons.
* **Interpolation:** The `interpolate` function uses linear interpolation.  If you need a different interpolation method, you'll have to modify the function.
* **Controlled Component:** The `TextInput` is controlled via `setNativeProps`.  This is generally less preferred in React Native.  A more idiomatic approach would be to use a state variable to manage the text input's value.  However, for this specific use case, it's a trade-off made for performance and simplicity.
