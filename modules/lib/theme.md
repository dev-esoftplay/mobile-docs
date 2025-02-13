# LibTheme

This module provides a centralized way to manage your application's theme, including colors and styles. It uses global state (from `esoftplay`) to store the current theme and provides methods to switch between themes.  It also persists the theme selection using `AsyncStorage`.

## Installation

This component relies on `esoftplay` and `@react-native-async-storage/async-storage`. Ensure you have them installed:

```bash
npm install esoftplay @react-native-async-storage/async-storage
# or
yarn add esoftplay @react-native-async-storage/async-storage
```

## Usage

```javascript
import { LibTheme } from 'esoftplay/cache/lib/theme/import';

// Setting the theme:
LibTheme.setTheme('dark'); // or 'light'

// Accessing theme colors:
const backgroundColor = LibTheme._colorBackgroundPrimary();
const textColor = LibTheme._colorTextPrimary();

// In your components:
<View style={{ backgroundColor: LibTheme._colorBackgroundPrimary() }}>
  <Text style={{ color: LibTheme._colorTextPrimary() }}>Some text</Text>
</View>
```

## API

### `LibTheme` (Object with static methods)

*   `setTheme(themeName: string): void`: Sets the current theme.  Triggers a navigation reset and persists the theme to `AsyncStorage`.
*   `_barStyle(): string`: Returns the status bar style ("dark" or "light") based on the current theme.
*   `_colorPrimary(): string`: Returns the primary color based on the current theme.
*   `_colorAccent(): string`: Returns the accent color based on the current theme.
*   `_colorHeader(): string`: Returns the header background color based on the current theme.
*   `_colorHeaderText(): string`: Returns the header text color based on the current theme.
*   `_colorButtonPrimary(): string`: Returns the primary button background color.
*   `_colorButtonTextPrimary(): string`: Returns the primary button text color.
*   `_colorButtonSecondary(): string`: Returns the secondary button background color.
*   `_colorButtonTextSecondary(): string`: Returns the secondary button text color.
*   `_colorButtonTertiary(): string`: Returns the tertiary button background color.
*   `_colorButtonTextTertiary(): string`: Returns the tertiary button text color.
*   `_colorBackgroundPrimary(): string`: Returns the primary background color based on the current theme.
*   `_colorBackgroundSecondary(): string`: Returns the secondary background color based on the current theme.
*   `_colorBackgroundTertiary(): string`: Returns the tertiary background color based on the current theme.
*   `_colorBackgroundCardPrimary(): string`: Returns the primary card background color based on the current theme.
*   `_colorBackgroundCardSecondary(): string`: Returns the secondary card background color based on the current theme.
*   `_colorBackgroundCardTertiary(): string`: Returns the tertiary card background color based on the current theme.
*   `_colorTextPrimary(): string`: Returns the primary text color based on the current theme.
*   `_colorTextSecondary(): string`: Returns the secondary text color based on the current theme.
*   `_colorTextTertiary(): string`: Returns the tertiary text color based on the current theme.
*   `colors(colors: string[]): string`: A helper function that takes an array of colors (one for each theme) and returns the appropriate color based on the current theme.

## Functionality

1. **Global State:** The module uses `useGlobalState` from `esoftplay` to store the current theme name.  This allows the theme to be accessed and changed from any component.

2. **Theme Persistence:** The selected theme is persisted using `AsyncStorage` so that it is preserved across app restarts.

3. **Theme Switching:** The `setTheme` function allows you to switch between themes.  It updates the global state, triggers a navigation reset (using `LibNavigation.reset()`), and saves the theme to `AsyncStorage`.

4. **Color Accessors:** The module provides a set of functions (e.g., `_colorPrimary`, `_colorBackgroundPrimary`) for accessing theme-specific colors. These functions use the `colors` helper function to retrieve the correct color based on the current theme.

5. **`colors` Helper Function:** The `colors` function takes an array of colors, where each element corresponds to a theme.  It returns the color associated with the currently active theme.  It also handles cases where the theme index might be out of bounds.

## Important Considerations

*   **`esoftplay` Dependency:** This module relies on `esoftplay` for global state management and navigation.
*   **`AsyncStorage` Dependency:** The theme selection is persisted using `AsyncStorage`.
*   **Theme Configuration:** The available themes and their associated colors are defined in your `esp.config('theme')` configuration.  The `LibTheme` module reads this configuration to determine the available themes.
*   **Navigation Reset:** The `setTheme` function calls `LibNavigation.reset()`.  This is likely done to ensure that all components re-render with the new theme applied.  Be aware of the implications of resetting navigation, as it might affect your app's navigation state.
*   **Color Accessors:** The `_color...` functions are named with a leading underscore to indicate that they are intended for internal use within the `LibTheme` module.  However, they are exported and can be used in your components.
*   **Customization:**  You can easily add more theme colors and styles to this module as your application's design evolves.
*   **Performance:** Accessing global state and `AsyncStorage` is generally performant.  However, if you have a very large number of components that depend on the theme, you might want to consider further optimizations (e.g., using React Context or a more sophisticated state management solution).
* **Error Handling:** The `colors` function provides basic error handling for out-of-bounds theme indices.  You might want to add more robust error handling or logging.
* **Naming Conventions:** The naming conventions used for the color accessors (e.g., `_colorBackgroundPrimary`) are common and help to make your code more readable.  The leading underscore is a convention to indicate that these functions are primarily for internal use within the `LibTheme` module, although they are still exported.
* **Scalability:** This approach to theme management is highly scalable.  As your app grows, you can easily add more themes, colors, and styles to this module.
