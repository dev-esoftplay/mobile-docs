# LibIcon

This component provides a convenient way to use icons from various icon sets within your React Native application. It wraps icons from `@expo/vector-icons` and provides a consistent interface for using them.

## Installation

This component relies on `@expo/vector-icons` and `esoftplay`. Ensure they are installed:

```bash
expo install @expo/vector-icons
npm install esoftplay
# or
yarn add @expo/vector-icons
yarn add esoftplay
```

## Usage

```javascript
import LibIcon from './your-lib-icon-file'; // Replace with the correct path

// Using MaterialCommunityIcons (default):
<LibIcon name="check" size={30} color="blue" />

// Using Ionicons:
LibIcon.Ionicons({ name: "ios-add", size: 40, color: "red" });

// Using AntDesign:
LibIcon.AntDesign({ name: "plus", size: 25, color: "green" });

// ... other icon sets
```

## API

### `LibIcon` (Component)

The main component for rendering icons.  It defaults to using `MaterialCommunityIcons`.

### Static Methods (Icon Set Renderers)

The `LibIcon` component provides static methods for rendering icons from specific icon sets.  Each method accepts props specific to that icon set.

#### **`Ionicons(props: LibIoniconsProps): any`**
Renders an icon from the `Ionicons` set.
#### **`AntDesign(props: LibAntDesignIconProps): any`**
Renders an icon from the `AntDesign` set.
#### **`EvilIcons(props: LibEvilIconsIconProps): any`**
Renders an icon from the `EvilIcons` set.
#### **`Feather(props: LibFeatherIconProps): any`**
Renders an icon from the `Feather` set.
#### **`FontAwesome(props: LibFontAwesomeIconProps): any`**
Renders an icon from the `FontAwesome` set.
#### **`Fontisto(props: LibFontistoIconProps): any`**
Renders an icon from the `Fontisto` set.
#### **`Foundation(props: LibFoundationIconProps): any`**
Renders an icon from the `Foundation` set.
#### **`Octicons(props: LibOcticonsIconProps): any`**
Renders an icon from the `Octicons` set.
#### **`Zocial(props: LibZocialIconProps): any`**
Renders an icon from the `Zocial` set.
#### **`MaterialIcons(props: LibMaterialIconsIconProps): any`**
Renders an icon from the `MaterialIcons` set.
#### **`SimpleLineIcons(props: LibSimpleLineIconProps): any`**
Renders an icon from the `SimpleLineIcons` set.
#### **`EntypoIcons(props: LibEntypoIconProps): any`**
Renders an icon from the `Entypo` set.

### Interfaces (Props)

Each icon set has a corresponding interface that defines the valid props for that icon.  These interfaces are based on the types provided by `@expo/vector-icons/build/esoftplay_icons`.

*   **`LibAntDesignIconProps`:** Props for `AntDesign` icons.
*   **`LibEvilIconsIconProps`:** Props for `EvilIcons` icons.
*   **`LibFeatherIconProps`:** Props for `Feather` icons.
*   **`LibFontAwesomeIconProps`:** Props for `FontAwesome` icons.
*   **`LibFontistoIconProps`:** Props for `Fontisto` icons.
*   **`LibFoundationIconProps`:** Props for `Foundation` icons.
*   **`LibMaterialIconsIconProps`:** Props for `MaterialIcons` icons.
*   **`LibEntypoIconProps`:** Props for `Entypo` icons.
*   **`LibOcticonsIconProps`:** Props for `Octicons` icons.
*   **`LibZocialIconProps`:** Props for `Zocial` icons.
*   **`LibSimpleLineIconProps`:** Props for `SimpleLineIcons` icons.
*   **`LibIoniconsProps`:** Props for `Ionicons` icons.
*   **`LibIconProps`:** Props for the default `MaterialCommunityIcons` icon.

### `LibIconState`

An empty interface, currently not used.

## Functionality

This component acts as a wrapper around the icon sets from `@expo/vector-icons`. It simplifies the usage of icons by providing a consistent interface and reducing boilerplate code.  It also sets a default size and color for the icons.

## Important Considerations

*   **Icon Sets:**  You must import the desired icon set's types from `@expo/vector-icons/build/esoftplay_icons`.
*   **Props:**  Refer to the documentation for `@expo/vector-icons` for the available icons and their props for each icon set.  The interfaces provided by `LibIcon` are based on those types.
*   **Default Styling:** The component sets a default size (23) and color (#222) for the icons.  You can override these by providing your own `size` and `color` props.
*   **Styling:**  You can apply additional styles to the icons using the `style` prop.
*   **Consistency:** Using this component promotes consistency in icon usage throughout your application.
*   **Extensibility:**  You can easily add support for other icon sets by creating new static methods and interfaces.
* **Type Safety:** The use of interfaces ensures type safety when using the icons.  This helps to prevent errors and improves code maintainability.
* **Default Size and Color:** The component provides default values for the `size` and `color` props.  This simplifies the usage of the icons and ensures a consistent look and feel.
* **Styling:**  The `style` prop allows you to apply additional styles to the icons.  This gives you flexibility to customize the appearance of the icons.
* **Component Structure:** The component uses a class component structure.
* **Static Methods:** The static methods provide a convenient way to render icons from different icon sets.
* **Icon Set Specific Props:** Each static method accepts props that are specific to the corresponding icon set.  This allows you to use all the features and customization options provided by the underlying icon library.
* **Wrapper Component:** The `LibIcon` component acts as a wrapper around the `@expo/vector-icons` components.  This provides a consistent API and simplifies the usage of icons in your application.  It also allows you to easily switch between different icon sets if needed.
* **Maintainability:**  Using this component makes your code more maintainable.  If you need to change the underlying icon library, you only need to update the `LibIcon` component, rather than updating all the icon usages in your application.