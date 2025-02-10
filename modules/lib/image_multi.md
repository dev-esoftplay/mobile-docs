# LibImage_multi

This component provides a multi-image selection interface using `expo-media-library`. It allows users to select multiple images from their device's photo library, with options to filter by media subtype and limit the number of selections.

## Installation

This component relies on `expo-media-library` and `esoftplay`. Ensure you have them installed:

```bash
npm install expo-media-library esoftplay
# or
yarn add expo-media-library esoftplay
```

## Usage

```javascript

// Navigating to the multi-image selector:
esp.mod("lib/navigation").navigateForResult("lib/image_multi", {
  max: 0, // Optional: Maximum number of selections (0 for unlimited)
  mediaSubtype: MediaLibrary.MediaType.photo // Optional: Media subtype to filter (e.g., MediaLibrary.MediaType.photo, MediaLibrary.MediaType.video)
}).then((result) => {
  if (result) {
    console.log("Selected images:", result);
    // Use the selected images (array of assets)
  } else {
    console.log("No images were selected or the operation was cancelled.");
  }
})
```

## Props

This component doesn't receive props directly. Instead, `max` and `mediaSubtype` are passed via navigation arguments using `esp.mod("lib/navigation").navigate`.

## Functionality

1. **Permissions:** The component requests permissions to access the device's media library using `expo-media-library`.

2. **Image Loading:** Images are loaded from the device's library in batches of 50 using `MediaLibrary.getAssetsAsync`.  Pagination is implemented using the `after` cursor.

3. **Media Subtype Filtering:** The `mediaSubtype` argument is used to filter the displayed images (e.g., to show only photos or only videos).

4. **Selection:** Users can tap on images to select or deselect them.

5. **Maximum Selections:** The `max` argument limits the number of images that can be selected.  A value of 0 means unlimited selections.

6. **Selected Count Display:** The number of selected images is displayed in the header.

7. **Confirmation:** The "Check" button allows users to confirm their selections and return the selected images via `LibNavigation.sendBackResult`.

8. **Close Button:** The "Close" button allows users to cancel the selection process.

9. **Image Display:** Images are displayed in a two-column grid using a `FlatList`.

10. **Loading Indicator:** A loading indicator is shown while images are being loaded.

## Important Considerations

* **Dependencies:** Ensure that `expo-media-library` and `esoftplay` are correctly installed and linked.
* **Navigation:** The component relies on `esp.mod("lib/navigation")` for navigation and passing arguments.
* **Permissions:** The component requests necessary permissions to access the media library.
* **Pagination:** The component loads images in batches and implements pagination to handle large libraries.
* **Selection Limit:** The `max` argument allows you to control the maximum number of selections.
* **Image Display:** The images are displayed in a grid with equal height and width.
* **Performance:** Loading large image libraries can be resource-intensive.  The component uses pagination and other optimizations to improve performance.  Consider further optimizations if necessary (e.g., caching, virtualization).
* **Accessibility:** Consider accessibility implications, such as providing alternative text for images and ensuring that the selection state is clearly indicated for users with visual impairments.
* **Error Handling:**  The component doesn't include explicit error handling for permission issues or other potential problems.  Consider adding more robust error handling to make the component more reliable.
* **Customization:**  The appearance of the image grid, selection indicators, and header can be customized further.