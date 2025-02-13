# LibTextstyle

This component provides a convenient way to apply pre-defined text styles to your text elements. It offers a range of styles based on Material Design and iOS typography guidelines.

## Installation

This component does not have any external dependencies beyond React Native itself.

## Usage

```javascript
import { LibTextstyle } from 'esoftplay/cache/lib/textstyle/import';

// Using a predefined text style:
<LibTextstyle textStyle="title1" text="This is a title" />

// With additional styling:
<LibTextstyle textStyle="body" text="This is body text" style={{ color: 'gray' }} />

// Using children instead of the 'text' prop:
<LibTextstyle textStyle="caption1">This is caption text</LibTextstyle>

// Setting numberOfLines and ellipsizeMode:
<LibTextstyle textStyle="body" text="This is long text that might be truncated" numberOfLines={2} ellipsizeMode="tail" />
```

## Props

| Prop             | Type        | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| ---------------- | ----------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `textStyle`       | `"largeTitle" \| "title1" \| "title2" \| "title3" \| "headline" \| "body" \| "callout" \| "subhead" \| "footnote" \| "caption1" \| "caption2" \| "m_h1" \| "m_h2" \| "m_h3" \| "m_h4" \| "m_h5" \| "m_h6" \| "m_subtitle1" \| "m_subtitle2" \| "m_body1" \| "m_body2" \| "m_button" \| "m_caption" \| "m_overline"` | Yes      | The name of the predefined text style to apply.                                                                                                                                                                                                                                                                                                                                                                                    |
| `style`          | `TextStyle` | No       | Additional styles to apply to the text.  These styles will be merged with the predefined style.                                                                                                                                                                                                                                                                                                                                                             |
| `children`       | `string`    | No       | The text content of the element.  Can be used instead of the `text` prop.                                                                                                                                                                                                                                                                                                                                                                             |
| `numberOfLines`  | `number`    | No       | The maximum number of lines to display.  If the text exceeds this number of lines, it will be truncated.                                                                                                                                                                                                                                                                                                                                                       |
| `ellipsizeMode`  | `string`    | No       | How to truncate the text if it exceeds the available space.  Options are `"head"`, `"middle"`, `"tail"`, and `"clip"`. Defaults to `"tail"`.                                                                                                                                                                                                                                                                                                                                                        |
| `text`           | `string`    | No       | The text content of the element. Can be used instead of the `children` prop.                                                                                                                                                                                                                                                                                                                                                                             |

## Functionality

1. **Predefined Styles:** The component defines a set of predefined text styles in the `styles` object. These styles are based on Material Design and iOS typography guidelines.

2. **Style Application:** The `textStyle` prop determines which predefined style is applied to the text.

3. **Additional Styling:** The `style` prop allows you to add additional styles to the text, which will be merged with the predefined style.

4. **Text Content:** The `text` or `children` prop provides the text content to be displayed.

5. **Scaling:** The component includes a `scale` property (currently set to 1) that could be used to scale the text size.  However, this functionality isn't actively used in the provided code.

## Important Considerations

*   **Predefined Styles:** The available `textStyle` options provide a good starting point for styling your text. You can customize these styles further using the `style` prop.
*   **Material Design and iOS Guidelines:** The predefined styles are based on Material Design and iOS typography guidelines, so they should provide a consistent look and feel on different platforms.
*   **Additional Styling:** The `style` prop is essential for customizing the appearance of your text beyond the predefined styles.
*   **`numberOfLines` and `ellipsizeMode`:** These props are useful for handling long text that might need to be truncated.
*   **`text` vs. `children`:** You can use either the `text` prop or the `children` prop to provide the text content.
*   **Scaling (Currently Unused):** The `scale` property and the code that multiplies the font size and letter spacing by this scale are present, but the `scale` variable is currently hardcoded to 1. If you want to implement text scaling, you can modify this.
* **Style Merging:** The component correctly merges the predefined styles with any additional styles provided in the `style` prop.  It handles both object styles and flattened styles (using `StyleSheet.flatten`).
* **String Conversion:** The component uses `String()` to ensure that the text content is always treated as a string, even if it's a number or other non-string value. This prevents potential errors.
