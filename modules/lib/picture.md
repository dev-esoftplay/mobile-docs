```markdown
# LibPicture

This component displays an image, handling both local assets (from `esoftplay`) and remote URIs. It uses `expo-image` for optimized image loading and caching.

## Installation

This component relies on `expo-image` and `esoftplay`. Ensure you have them installed:

```bash
npm install expo-image esoftplay
# or
yarn add expo-image esoftplay
```

## Usage

```javascript
import { LibPicture } from 'esoftplay/cache/lib/picture/import';

// Displaying a remote image:
<LibPicture
  source={{ uri: 'https://picsum.photos/seed/picsum/1000/500' }}
  style={{ width: 200, height: 150 }}
/>

// Displaying a local asset:
<LibPicture
  source={esp.assets("my_image.png")} // Assuming 'my_image.png' is in your assets
  style={{ width: 100, height: 100 }}
/>

// With resizeMode:
<LibPicture
  source={{ uri: 'image_url' }}
  style={{ width: 200, height: 150, resizeMode: 'contain' }}
/>

// With onError:
<LibPicture
  source={{ uri: 'image_url' }}
  style={{ width: 200, height: 150 }}
  onError={() => console.log('Image failed to load')}
/>
```

## Props

| Prop         | Type        | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| ----------- | ----------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `source`    | `LibPictureSource \| any`       | Yes      | The source of the image. Can be an object with a `uri` property (for both remote and local assets), or any other format that `expo-image` accepts. If the `uri` does *not* start with `http` or `https`, it is treated as a local asset and resolved using `esp.assets`.                                                                                                                                                                                                                                                                                                                                                                |
| `style`     | `ImageStyle` | Yes      | Style for the image (including `width` and `height`).  `resizeMode` can be set here.                                                                                                                                                                                                                                                                                                                                                                |
| `resizeMode` | `"contain" \| "cover"` | No       | How the image should be resized to fit its container.  Defaults to `"cover"`.  Can also be set in the `style` prop.  The value in the `style` prop takes precedence.  to:
| `onError`  | `() => void` | No       | Optional callback function called if the image fails to load.                                                                                                                                                                                                                                                                                                                                                                                    |

## Functionality

1. **Source Handling:** The component checks if the `uri` in the `source` prop starts with `http`. If not, it treats it as a local asset and uses `esp.assets` to resolve the path.

2. **`expo-image` Component:** The component renders an `Image` component from `expo-image`, passing the processed `source`, `style`, `contentFit` (derived from `resizeMode`), `allowDownscaling`, and `cachePolicy` props.

3. **Resize Mode:** The `resizeMode` prop is used to set the `contentFit` property of the `expo-image` component.  It defaults to `cover`.  If `resizeMode` is specified both in the `style` prop and as a separate prop, the value in the `style` prop takes precedence.

4. **Error Handling:** The `onError` prop is passed to the `expo-image` component, allowing you to handle image loading errors.

## Important Considerations

*   **`expo-image` Dependency:** Ensure that `expo-image` is correctly installed and linked.
*   **`esoftplay` Dependency:** This component uses `esoftplay` for asset management.  Make sure `esoftplay` is configured in your project.
*   **Local Assets:**  Local assets should be placed in your project's assets directory and referenced by their filename (without the path). `esp.assets` will resolve the correct path to the asset.
*   **Remote Images:** Remote images should be specified using a full URI (starting with `http` or `https`).
*   **Resize Mode:** The `resizeMode` prop controls how the image is scaled to fit its container.  `cover` will fill the container while maintaining the image's aspect ratio (potentially cropping parts of the image).  `contain` will ensure the entire image is visible within the container, potentially adding letterboxing or pillarboxing.
*   **Caching:** The `cachePolicy` is set to `memory-disk` to leverage `expo-image`'s caching capabilities.
*   **Error Handling:** The `onError` callback can be used to handle image loading errors, such as displaying a placeholder image or a message to the user.
*   **Performance:** `expo-image` is optimized for performance, but for very large images or a large number of images, you might need to consider further optimizations, such as using a placeholder while the image loads or lazy loading images that are not immediately visible.
* **Accessibility:**  Provide an `accessibilityLabel` prop to the `LibPicture` component to describe the image for users with visual impairments.  If the image is purely decorative, you can set `accessible={false}`.  If the image conveys important information, make sure the `accessibilityLabel` accurately describes the image's content.
