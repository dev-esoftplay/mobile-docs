# LibGallery

This component displays an image gallery using `react-native-awesome-gallery`. It supports displaying a single image or multiple images, with zoom functionality and a close button.

## Installation

This component relies on `react-native-awesome-gallery` and `esoftplay`. Ensure you have them installed:

```bash
npm install react-native-awesome-gallery esoftplay
# or
yarn add react-native-awesome-gallery esoftplay
```

## Usage

```javascript
// Displaying a single image:
esp.mod("lib/navigation").navigate("lib/gallery", { image: "https://picsum.photos/seed/picsum/200/300" });

// Displaying multiple images:
const images = [
  {
    image:"https://picsum.photos/seed/picsum/1000/500",
    title:"Image 1"
  },
  {
    image:"https://picsum.photos/seed/picsum/1000/500",
    title:"Image 2"
  },
]

esp.mod("lib/navigation").navigate("lib/gallery", { images, index: 1 }); // Start at index 1

```

## Props

This component doesn't receive props directly. Instead, the `images` (or `image` for a single image) and `index` are passed via navigation arguments using `esp.mod("lib/navigation").navigate`.

## Functionality

1. **Navigation Arguments:** The component retrieves the `images` (or `image`) and `index` from the navigation arguments using `esp.mod("lib/navigation").getArgs`.

2. **Image Data:** If `images` is provided, it's used directly. If `images` is empty *and* `image` is provided, an array is created with a single image object.  If neither are provided, the gallery will likely not display correctly.

3. **Gallery Component:** The `Gallery` component from `react-native-awesome-gallery` is used to display the images.

4. **Swipe to Close:** The `onSwipeToClose` prop is used to handle swipe gestures to close the gallery.  It only calls `LibNavigation.back()` if the image is not scaled (i.e., `scale == 1`).

5. **Zoom:** The gallery supports zooming with `maxScale` and `doubleTapScale`. The current scale is tracked using the `scale` ref.

6. **Close Button:** A `Pressable` component is used as a close button.  It calls `LibNavigation.back()` when pressed.

7. **Styling:** The gallery and close button have basic styling applied.

## Important Considerations

* **`react-native-awesome-gallery` Dependency:** This component relies on the `react-native-awesome-gallery` library. Ensure it is correctly installed and linked.
* **`esoftplay` Dependency:** This component depends on `esoftplay` for navigation and icon display.  Make sure it is correctly set up in your project.
* **Navigation:** The component relies on `esp.mod("lib/navigation")` for navigation and passing arguments.
* **Image URLs:** Make sure the `image` URLs in the `images` array are valid and accessible.
* **Single Image vs. Multiple Images:** The component handles both single image and multiple image scenarios.
* **Zoom Handling:** The `onScaleChange` prop is used to track the zoom level, which is then used to control whether swipe-to-close is enabled.
* **Close Button:** The close button provides a standard way to exit the gallery.
* **Styling:** The styling is basic.  You'll likely want to customize the appearance of the gallery and close button to match your app's design.
* **Accessibility:** Consider adding accessibility features to the gallery, such as proper ARIA attributes and keyboard navigation support.  Ensure that users with disabilities can interact with the gallery and understand the content being displayed.  Provide alternative text for images if possible.
* **Error Handling:**  The component doesn't include explicit error handling for invalid image URLs or other potential issues.  You might want to add error handling to make the component more robust.
