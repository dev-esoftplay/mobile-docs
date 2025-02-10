# LibDatepicker

This component provides a customizable date picker for React Native. It supports selecting dates, months, or years, with configurable minimum and maximum dates, localized month names, and a callback for date changes.

## Installation

This component relies on `esoftplay` and `expo-linear-gradient`. Ensure you have these installed in your project.

```bash
npm install esoftplay expo-linear-gradient
# or
yarn add esoftplay expo-linear-gradient
```

## Usage

```javascript
import { LibDatepicker } from 'esoftplay/cache/lib/datepicker/import';

<LibDatepicker
  minDate="2020-01-01"
  maxDate="2024-12-31"
  selectedDate="2023-10-27"
  onDateChange={(date) => console.log("Selected Date:", date)}
  type="date" // Optional: "date", "month", or "year" (defaults to "date")
  monthsDisplay={["Jan", "Feb", "Mar", /* ... */]} // Optional: Custom month names
/>
```

## Props

| Prop            | Type                 | Required | Default | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| --------------- | -------------------- | -------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `minDate`       | `DateFormat`         | Yes      |         | The minimum selectable date in "YYYY-MM-DD" format.                                                                                                                                                                                                                                                                                                                                                                                                                   |
| `maxDate`       | `DateFormat`         | Yes      |         | The maximum selectable date in "YYYY-MM-DD" format.                                                                                                                                                                                                                                                                                                                                                                                                                   |
| `monthsDisplay` | `string[]`           | No       |         | An array of month names. If not provided, localized month names from `esoftplay` are used.                                                                                                                                                                                                                                                                                                                                                                            |
| `selectedDate`  | `DateFormat`         | Yes      | `minDate` | The initially selected date in "YYYY-MM-DD" format.                                                                                                                                                                                                                                                                                                                                                                                                                   |
| `onDateChange`  | `(date: DateFormat) => void` | Yes      |         | Callback function that is called when the selected date changes. Receives the new date in "YYYY-MM-DD" format as an argument.                                                                                                                                                                                                                                                                                                                                                     |
| `type`          | `"date" \| "month" \| "year"` | No       | `"date"` | The type of picker to display.  "date" shows day, month, and year. "month" shows month and year. "year" shows only the year.                                                                                                                                                                                                                                                                                                                                                                                                                                                  |

## Functionality

1. **Scroll Pickers:** The component uses `LibScrollpicker` (from `esoftplay`) for the year, month, and day pickers.

2. **Date Range:** The `minDate` and `maxDate` props define the selectable date range.

3. **Localized Months:** The `monthsDisplay` prop allows customizing month names. If not provided, the component attempts to use localized month names using `esp.lang("lib/datepicker", "monthX")` from `esoftplay`.

4. **Date Selection:** The `selectedDate` prop sets the initial selection.

5. **Date Change Callback:** The `onDateChange` prop is a callback function that is called whenever the user selects a new date.

6. **Picker Type:** The `type` prop controls which pickers are displayed (date, month, or year).

7. **Linear Gradient:** A linear gradient is used to create a visual effect on the scroll pickers.

8. **Loading State:** A loading indicator (`LibLoading` from `esoftplay`) is displayed while the component initializes (specifically, while the `dates` array is empty, which happens during the initial setup).

## `DateFormat` Type

The `DateFormat` type is defined as:

```typescript
type DateFormat = `${number}-${number}-${number}`
```

This ensures that dates are always in the "YYYY-MM-DD" format.

## Important Considerations

* **`esoftplay` Dependency:** This component is tightly coupled to the `esoftplay` library. Make sure it is correctly set up in your project. All modules (scrollpicker, style, toast) are accessed via `esp`.
* **`expo-linear-gradient`:** This library is used for the gradient effect. Ensure it's installed and linked correctly.
* **Date Handling:** The component handles date calculations within the specified range.
* **Localization:** The component attempts to use localized month names.  Make sure your `esoftplay` localization is configured correctly if you want to use this feature.
* **Type Safety:** The use of the `DateFormat` type enhances type safety.
* **Refs:** Refs (`refYear`, `refMonth`, `refDate`) are used to interact with the `LibScrollpicker` components.
* **Loading State:** The loading state ensures that the component doesn't try to render the pickers before the data is ready.
* **Component Structure:** The component is structured to handle different picker types (date, month, year) efficiently. The `showDateView`, `showMonthView`, and `showYearView` variables control the visibility of each picker.
* **Accessibility:** Consider adding accessibility features to the date picker, such as labels and appropriate ARIA attributes.
