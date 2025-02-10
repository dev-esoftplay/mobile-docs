# LibImage_crop

This component provides an image cropping tool using `expo-image-manipulator` and `react-native-pan-pinch-view`. It allows users to select a cropping area, resize the image, and save the cropped image.

## Installation

This component relies on `expo-image-manipulator`, `react-native-pan-pinch-view`, and `esoftplay`. Ensure you have them installed:

```bash
npm install expo-image-manipulator react-native-pan-pinch-view esoftplay
# or
yarn add expo-image-manipulator react-native-pan-pinch-view esoftplay
```

## Usage

```javascript

// Navigating to the image cropper:
esp.mod("lib/navigation").navigateForResult("lib/image_crop", {
  image: "image_uri", // The URI of the image to crop
  ratio: "3:2", // Optional: The desired aspect ratio (e.g., "16:9", "1:1")
  forceCrop: false, // Optional: If true, the "Save" button only appears after a crop operation.
  message: "Optional: Message to display in the hint box" // Optional: Hint message
})
.then((result)=>{
  // Handling the result after cropping:
  if (result) {
    console.log("Cropped image URI:", result.uri);
    console.log("Cropped image width:", result.width);
    console.log("Cropped image height:", result.height);
    // Use the cropped image URI
  } else {
    console.log("No image was cropped or the operation was cancelled.");
  }
})

```

## Props

This component doesn't receive props directly. Instead, the `image`, `ratio`, `forceCrop`, and `message` are passed via navigation arguments using `esp.mod("lib/navigation").navigate`.

## Functionality

1. **Navigation Arguments:** The component retrieves `image`, `ratio`, `forceCrop`, and `message` from navigation arguments.

2. **Image Resizing:** The component resizes the image to a maximum size of 900px while maintaining aspect ratio using `expo-image-manipulator`.

3. **Aspect Ratio:** The `ratio` prop (e.g., "3:2") determines the aspect ratio of the cropping area.  If not provided, it defaults to 3:2.

4. **Cropping Area:** A dashed border represents the cropping area.  Its size is determined by the `ratio`.

5. **Pan and Pinch:** The `react-native-pan-pinch-view` library allows users to pan and pinch the image within the cropping area.

6. **Cropping:** The `capture` function calculates the cropping region based on the position and size of the cropping area and the image's actual dimensions. It then uses `expo-image-manipulator` to crop the image.

7. **Reset:** The `reset` function restarts the image cropper with the original image.

8. **Save:** The "Save" button saves the cropped image and sends the URI, width and height back using `LibNavigation.sendBackResult`. The button is only shown if `forceCrop` is false, or if `forceCrop` is true and at least one crop operation has been performed.

9. **Hint Message:** A hint message is displayed at the bottom, which can be customized with the `message` parameter.

10. **Status Bar:** The component uses `LibStatusbar` to set the status bar style to light.

11. **Icons:** `LibIcon` is used for icons (close, undo, crop).

12. **Progress Indicator:** `LibProgress` is used to show a loading indicator during image resizing and cropping.

13. **Toast:** `LibToastProperty` is used to display error messages.

## Important Considerations

* **Dependencies:** Ensure all required libraries (`expo-image-manipulator`, `react-native-pan-pinch-view`, and `esoftplay`) are correctly installed and linked.
* **Navigation:** The component relies on `esp.mod("lib/navigation")` for navigation and passing arguments.
* **Image URI:** The `image` argument should be a valid image URI.
* **Aspect Ratio Handling:** The component correctly calculates and applies the desired aspect ratio for the cropping area.
* **Pan and Pinch:** The `react-native-pan-pinch-view` library provides the pan and pinch functionality.
* **Cropping Logic:** The `capture` function accurately calculates the cropping region and uses `expo-image-manipulator` to perform the cropping.
* **Error Handling:** Basic error handling is included (e.g., showing a toast message if cropping fails).  Consider adding more robust error handling.
* **UI/UX:** The UI provides a clear cropping area, reset and save buttons, and a hint message. Consider further UI/UX improvements.
* **Accessibility:** Consider accessibility implications, such as providing alternative text for the image and ensuring that the cropping area is clearly indicated for users with visual impairments.
