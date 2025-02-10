```markdown
# LibIcon

This component provides a unified way to use icons from various icon sets, including MaterialCommunityIcons, AntDesign, Entypo, EvilIcons, Feather, FontAwesome, Fontisto, Foundation, Ionicons, MaterialIcons, Octicons, SimpleLineIcons, and Zocial. It simplifies icon usage by providing static methods for each icon set.

## Installation

This component relies on `@expo/vector-icons` and `esoftplay`. Ensure you have them installed:

```bash
npm install @expo/vector-icons esoftplay
# or
yarn add @expo/vector-icons esoftplay
```

## Usage

```javascript
import { LibIcon } from 'esoftplay/cache/lib/icon/import';

// Using MaterialCommunityIcons (the default):
<LibIcon name="check" size={30} color="blue" />

// Using AntDesign:
LibIcon.AntDesign({ name: "arrowright", size: 20, color: "red" });

// Using Ionicons:
LibIcon.Ionicons({ name: "ios-checkmark-circle-outline", size: 25, color: "green" });

// ... other icon sets ...
```

## API

### Component Props (for MaterialCommunityIcons - the default)

| Prop    | Type                 | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| ------- | -------------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`  | `MaterialCommunityIconsTypes` | Yes      | The name of the icon (see `@expo/vector-icons/build/esoftplay_icons` for available icons).                                                                                                                                                                                                                                                                                                                        |
| `size`  | `number`             | No       | The size of the icon (defaults to 23).                                                                                                                                                                                                                                                                                                                                                                                      |
| `color` | `string`             | No       | The color of the icon (defaults to `#222`).                                                                                                                                                                                                                                                                                                                                                                                    |
| `style` | `TextStyle`          | No       | Additional styles for the icon.                                                                                                                                                                                                                                                                                                                                                                                          |

### Static Methods (for other icon sets)

The `LibIcon` component provides static methods for each icon set:

* `LibIcon.AntDesign(props)`
* `LibIcon.EntypoIcons(props)`
* `LibIcon.EvilIcons(props)`
* `LibIcon.Feather(props)`
* `LibIcon.FontAwesome(props)`
* `LibIcon.Fontisto(props)`
* `LibIcon.Foundation(props)`
* `LibIcon.Ionicons(props)`
* `LibIcon.MaterialIcons(props)`
* `LibIcon.Octicons(props)`
* `LibIcon.SimpleLineIcons(props)`
* `LibIcon.Zocial(props)`

Each of these methods accepts an object with the following properties:

| Prop    | Type                 | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| ------- | -------------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`  | `[IconSet]Types`      | Yes      | The name of the icon (see `@expo/vector-icons/build/esoftplay_icons` for available icons for the specific icon set).                                                                                                                                                                                                                                                                                                                        |
| `size`  | `number`             | No       | The size of the icon (defaults to 23).                                                                                                                                                                                                                                                                                                                                                                                      |
| `color` | `string`             | No       | The color of the icon (defaults to `#222`).                                                                                                                                                                                                                                                                                                                                                                                    |
| `style` | `TextStyle`          | No       | Additional styles for the icon.                                                                                                                                                                                                                                                                                                                                                                                          |

Where `[IconSet]Types` is the specific type for that icon set (e.g., `AntDesignTypes`, `EntypoTypes`, etc.).

## `LibIconStyle` Type

```typescript
export type LibIconStyle = MaterialCommunityIconsTypes
```

This type alias defines the type for the `name` prop of the default `LibIcon` component (MaterialCommunityIcons).

## Functionality

1. **Unified Icon Component:** The `LibIcon` component acts as a wrapper for different icon sets, providing a consistent way to use icons throughout your application.

2. **Static Methods:** The static methods provide a convenient way to access icons from different sets without needing to import each icon set individually.

3. **Default Icon Set:** MaterialCommunityIcons is the default icon set.  If you use the `LibIcon` component directly (without a static method), it will render a MaterialCommunityIcons icon.

4. **Size and Color:** The `size` and `color` props allow you to customize the appearance of the icons.

5. **Styling:** The `style` prop allows you to apply additional styles to the icons.

## Important Considerations

* **`@expo/vector-icons` Dependency:** This component relies on `@expo/vector-icons`. Ensure it is correctly installed and linked in your project.
* **`esoftplay` Dependency:** This component extends `LibComponent` from `esoftplay`.
* **Icon Set Types:** Pay close attention to the `[IconSet]Types` when using the static methods.  These types ensure that you are using valid icon names for each specific icon set.  Refer to the `@expo/vector-icons/build/esoftplay_icons` file for the complete list of available icons and their types.
* **Default Size and Color:** The default size is 23, and the default color is `#222`.  You can override these by providing the `size` and `color` props.
* **Styling:** The `style` prop allows you to apply additional styles to the icon, such as margins, padding, or other visual effects.
* **Performance:** Using vector icons is generally performant. However, for very large numbers of icons, consider optimizing by using icon fonts or caching strategies.
* **Accessibility:**  When using icons, ensure that they are accessible to users with disabilities.  If an icon conveys important information, provide alternative text (e.g., using `aria-label` or `accessible={true}` and `accessibilityLabel` props if you are using React Native's `Image` component to render the icon).  If the icon is purely decorative, you can set `aria-hidden={true}` or `accessible={false}`.
