# LibImage_shadow

This component creates an image with a shadow effect using a blurred image and a shadow image. It's designed to give a more realistic shadow appearance compared to standard shadow properties.

## Installation

This component relies on `esoftplay`. Ensure you have it installed:

```bash
npm install esoftplay
# or
yarn add esoftplay
```

## Usage

```javascript
import { LibImage_shadow } from 'esoftplay/cache/lib/image/import';

<LibImage_shadow
  source={{ uri: 'image_url' }}
  style={{ width: 200, height: 150 }}
  shadowRadius={0.2} // Optional: Shadow radius (relative to image height)
  blurRadius={10} // Optional: Blur radius for the shadow image
/>
```

## Props

| Prop         | Type        | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| ----------- | ----------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `style`     | `ImageStyle` | Yes      | Style for the image (including `width` and `height`).  Both width and height *must* be defined.                                                                                                                                                                                                                                                                                                                           |
| `source`    | `any`       | Yes      | The source of the image (e.g., URI, local file).                                                                                                                                                                                                                                                                                                                                                                |
| `shadowRadius` | `number`    | No       | The radius of the shadow. This value is relative to the image `height`.  A value of 0.15 (the default) means the shadow's extra height will be 15% of the image's height.                                                                                                                                                                                                                                                                                                                      |
| `blurRadius` | `number`    | No       | The blur radius for the blurred image used to create the shadow effect. Defaults to 8.                                                                                                                                                                                                                                                                                                                               |

## Functionality

1. **Required Dimensions:** The component requires both `width` and `height` to be defined in the `style` prop.  It throws an error if they are not provided.

2. **Shadow Height Calculation:** The height of the shadow is calculated based on the `shadowRadius` and the image `height`.

3. **View Layout:** A `View` is used to contain the image and the shadow effect. The `View`'s height is adjusted to accommodate the shadow.

4. **Blurred Image:** A blurred version of the image is displayed at the bottom of the container. This blurred image acts as the base for the shadow effect.

5. **Shadow Image:** A shadow image (`blur.png`) is used to create the actual shadow effect.  This image is stretched to cover the area below the blurred image. Make sure you have a `blur.png` image in your assets.

6. **Main Image:** The original image is displayed on top of the blurred image and the shadow image, creating the final effect.

## Important Considerations

* **`esoftplay` Dependency:** This component relies on `esoftplay` for image display and asset management. Ensure it is correctly set up in your project.
* **`blur.png` Asset:**  The component uses a `blur.png` image for the shadow.  You *must* include this image in your project's assets and make it accessible to `esoftplay`.
* **Dimensions:**  The `width` and `height` in the `style` prop are *required*.  The component will throw an error if they are not provided.
* **Shadow Radius:** The `shadowRadius` is relative to the image `height`.  Adjust this value to control the size of the shadow.
* **Blur Radius:** The `blurRadius` controls the blurriness of the shadow.
* **Performance:** Using multiple images for the shadow effect can impact performance, especially if you have many of these components.  Consider optimizing by caching the blurred image or using a different shadow technique if necessary.
* **Customization:** You can customize the `blur.png` image to change the appearance of the shadow.  You can also adjust the `shadowRadius` and `blurRadius` props to fine-tune the shadow effect.
* **Accessibility:** Consider accessibility implications.  If the shadow is purely decorative, you might not need to do anything.  However, if the shadow helps to distinguish the image from the background, you might want to consider alternative ways to provide that visual distinction for users who cannot see the shadow.  Ensure sufficient contrast between the image and its background.
