# UserLoading

This component displays a loading screen using an image background. It's typically used while the application is initializing or fetching data.

## Installation

This component relies on `esoftplay`. Ensure it is installed:

```bash
npm install esoftplay
# or
yarn add esoftplay
```

## Usage

```javascript
import { UserLoading } from 'esoftplay/cache/user/loading/import';

// In your app's main component or where you need a loading screen:
{isLoading && <UserLoading />}
```

## API

### `UserLoading` (Component)

Displays the loading screen.

## Props

This component does not take any props.

## Functionality

1.  **Image Background:** Uses an `ImageBackground` component to display a splash image (either a GIF or a PNG).

2.  **Asset Loading:** The image is loaded using `esp.assets('splash.gif')` or `esp.assets('splash.png')`. This suggests that the images are managed by `esoftplay`'s asset system.  It tries to load `splash.gif` first, and if that fails (presumably because the file doesn't exist), it falls back to `splash.png`.

3.  **Full Screen:** The `ImageBackground` is styled with `flex: 1` to fill the entire screen.

## Important Considerations

*   **`esoftplay` Dependencies:** Relies on `esoftplay`'s asset management (`esp.assets`).
*   **Splash Image:**  The component expects a `splash.gif` and a `splash.png` file to be available in your assets.  Make sure these files exist.
*   **Loading State:** This component is typically used in conjunction with a loading state variable.  You would conditionally render the `UserLoading` component while the app is loading and hide it once the loading is complete.
*   **Styling:**  The styling is minimal (just `flex: 1`).  You can customize it further if needed.
*   **Fallback Image:** The component uses a fallback image (`splash.png`) in case the GIF image (`splash.gif`) is not available. This is a good practice to ensure that there is always a loading screen displayed.
*   **Asset Management:** The use of `esp.assets` suggests that `esoftplay` provides a way to manage and load assets.  Make sure you understand how `esp.assets` works and how to add your splash images to your project's assets.
*   **Conditional Rendering:** The component is usually rendered conditionally based on a loading state. This is a common pattern for displaying a loading screen while data is being fetched or the application is initializing.
* **GIF vs. PNG:** The component prioritizes a GIF image (`splash.gif`) for the loading screen.  GIFs can be animated, which can provide a more engaging loading experience.  However, if a GIF is not available, it falls back to a static PNG image (`splash.png`).  This is a good approach to ensure that the loading screen is always displayed, even if the GIF is missing.
* **Full-Screen Background:** The `flex: 1` style ensures that the `ImageBackground` covers the entire screen, providing a full-screen loading experience.
* **Customization:** While the styling is minimal, you can customize the loading screen further by adding other elements, such as a loading indicator, text, or a logo.  You can also modify the styling of the `ImageBackground` to change the background color, resize mode, etc.
* **Performance:**  Using an image for a loading screen is generally efficient.  However, if you have a very complex animation or a very large image, it could potentially impact performance.  In such cases, consider optimizing the image or using a different approach for the loading screen.
