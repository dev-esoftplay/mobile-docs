# LibLoading

This component displays a centered loading indicator using `ActivityIndicator`. It provides a simple and consistent way to show a loading state in your application.

## Installation

This component relies on `esoftplay`. Ensure you have it installed:

```bash
npm install esoftplay
# or
yarn add esoftplay
```

## Usage

```javascript
import { LibLoading } from 'esoftplay/cache/lib/loading/import';

<LibLoading />
```

## Props

This component does not accept any props.

## Functionality

1. **Centered `ActivityIndicator`:** The component renders an `ActivityIndicator` centered both horizontally and vertically within a `View` that occupies the entire available space (`flex: 1`).

2. **Color and Size:** The `ActivityIndicator` uses the primary color from `LibStyle` (presumably defined in your `esoftplay` theme) and a size of `large`.

## Important Considerations

* **`esoftplay` Dependency:** This component depends on `esoftplay` for styling.  Make sure it is correctly set up in your project.
* **Styling:** The styling of the `ActivityIndicator` (specifically the color) is determined by `LibStyle.colorPrimary`.  You'll need to configure this color in your `esoftplay` theme.
* **Full-Screen Loading:** The `View` surrounding the `ActivityIndicator` has `flex: 1`, which means it will expand to fill its parent container.  This makes the loading indicator appear centered on the screen.  If you want the loading indicator to be a different size or in a different location, you'll need to adjust the styles accordingly.
* **Customization:** While the component itself doesn't take props, you can wrap it in a `View` and apply styles to that `View` to further customize the presentation of the loading indicator. For example:

```javascript
<View style={{ position: 'absolute', top: 0, left: 0, right: 0, bottom: 0, backgroundColor: 'rgba(0, 0, 0, 0.5)' }}>
  <LibLoading />
</View>
```

This would create a semi-transparent overlay with the loading indicator centered on top of it.

* **Alternatives:**  You could also use a different loading indicator component or create your own custom loading animation.  This component provides a simple default for common use cases.
* **Accessibility:**  When showing a loading indicator, it's a good practice to indicate the loading state to users with disabilities.  You can do this by setting the `aria-busy` attribute to `true` on a relevant element (e.g., the parent container) and providing an appropriate `aria-label` or `accessibilityLabel` that describes the loading state.  For example:

```javascript
<View
  style={{ flex: 1 }}
  aria-busy={true}
  accessibilityLabel="Loading..."
>
  <LibLoading />
</View>
```