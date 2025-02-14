# LibImage

This component provides a comprehensive image handling utility, including capturing images from the camera, selecting images from the gallery, cropping images, and uploading images to a server. It uses `expo-camera`, `expo-image-picker`, and `expo-image-manipulator` for image processing.

## Installation

This component relies on several Expo and `esoftplay` libraries. Ensure they are installed:

```bash
expo install expo-camera expo-image-picker expo-image-manipulator
npm install esoftplay
# or
yarn add expo-camera expo-image-picker expo-image-manipulator
yarn add esoftplay
```

## Usage

```javascript
import { LibImage } from 'esoftplay/cache/lib/image/import';

// To capture an image from the camera:
LibImage.fromCamera({ /* options */ }).then((imageUri) => {
  // Use the imageUri
});

// To select an image from the gallery:
LibImage.fromGallery({ /* options */ }).then((imageUri) => {
  // Use the imageUri
});

// To crop an image:
LibImage.showCropper(uri, forceCrop, ratio, message, (croppedImageUri) => {
  // Use the croppedImageUri
});

// To set the result (after cropping or other processing):
LibImage.setResult(imageUri);

```

## API

### `LibImage` (Class)

Provides static methods for image handling.

### Static Methods

#### **`setResult(image: string): void`:**
Sets the resulting image URI in the global state.  This is typically called after an image is selected, cropped, or processed.
#### **`show(): void`:**
Shows the image picker/camera interface.  This sets the `show` property in the global state to `true`.
#### **`hide(): void`:**
Hides the image picker/camera interface and resets the image-related #### state.  This sets the global state back to its initial state.
#### **`showCropper(uri: string, forceCrop: boolean, ratio: string, message: string, result: (x: any) => void): void`:**
Navigates to the image cropper screen (`lib/image_crop`).
* `uri`: The URI of the image to crop.
* `forceCrop`: Whether cropping is mandatory.
* `ratio`: The aspect ratio for cropping (e.g., "1:1", "4:3").
* `message`: Optional message to display on the cropper screen.
* `result`: Callback function that receives the cropped image URI.
#### **`fromCamera(options?: LibImageCameraOptions): Promise<string>`:**
Opens the camera to capture an image.  Returns a Promise that resolves with the image URI.
  *   `options`: Optional configurations for camera.
  *   `crop`: Cropping options (`LibImageCrop`).
  *   `maxDimension`: Maximum dimension of the image.
#### **`fromGallery(options?: LibImageGalleryOptions): Promise<string | string[]>`:**
Opens the image gallery to select an image(s). Returns a Promise that resolves with the image URI or an array of image URIs if multiple selections are allowed.
  *   `options`: Optional configurations for gallery.
  *   `crop`: Cropping options (`LibImageCrop`).
  *   `maxDimension`: Maximum dimension of the image.
  *   `multiple`: Whether multiple selections are allowed.
  *   `max`: Maximum number of images to select.
#### **`processImage(result: any, maxDimension?: number): Promise<string>`:**
Processes the selected or captured image (resizing and uploading). Returns a Promise that resolves with the uploaded image URL.
*   `result`: The image selection result from `ImagePicker`.
*   `maxDimension`: Maximum dimension of the image.

### Instance Methods

#### **`takePicture(): Promise<void>`:**
Takes a picture using the camera.

### Interfaces

#### **`LibImageCrop`:**
Defines the options for cropping an image.
* `ratio`: Aspect ratio for cropping (e.g., "1:1", "4:3").
* `forceCrop`: Whether cropping is mandatory.
* `message`: Optional message to display on the cropper screen.
####   **`LibImageProps`:**
Defines the props for the `LibImage` component.
* `show`: Whether to show the image picker/camera.
* `image`: The current image URI.
* `maxDimension`: Maximum dimension of the image.
#### **`LibImageState`:**
Defines the state for the `LibImage` component.
* `type`: Camera type ("back" or "front").
* `loading`: Whether the camera is taking a picture.
* `image`: The current image URI.
* `flashLight`: Flashlight status ("on" or "off").
#### **`LibImageGalleryOptions`:**
Defines the options for selecting images from the gallery.
* `crop`: Cropping options (`LibImageCrop`).
* `maxDimension`: Maximum dimension of the image.
* `multiple`: Whether multiple selections are allowed.
* `max`: Maximum number of images to select.
#### **`LibImageCameraOptions`:**
Defines the options for capturing images from the camera.
 * `crop`: Cropping options (`LibImageCrop`).
 * `maxDimension`: Maximum dimension of the image.

## Functionality

1.  **Image Selection:** Allows selecting images from the device's gallery or capturing them using the camera.

2.  **Image Cropping:** Supports cropping images to a specified aspect ratio.

3.  **Image Resizing:** Resizes images to a maximum dimension before uploading.

4.  **Image Uploading:** Uploads the processed image to a server using `LibCurl`.

5.  **Camera Control:** Provides controls for switching between front and back cameras and toggling the flashlight.

6.  **UI:** Displays the camera preview and provides buttons for capturing images, switching cameras, toggling the flashlight, and confirming or canceling the image selection.

7.  **Permissions:** Requests necessary permissions for accessing the camera and image gallery.

8.  **Global State Management:** Uses global state to manage the visibility of the image picker/camera and the currently selected image.

## Important Considerations

*   **Permissions:** The component requests camera and media library permissions.  Handle permission denial gracefully.
*   **Image Processing:** Images are resized using `expo-image-manipulator` before uploading.
*   **Uploading:** Images are uploaded using `LibCurl`.  Make sure your server endpoint is configured correctly.
*   **Cropping:** The component integrates with a separate image cropping screen (`lib/image_crop`).
*   **Global State:** Uses global state for managing the component's visibility and selected image.
*   **Dependencies:** Relies on several Expo and `esoftplay` libraries.
*   **Platform Specifics:** Handles platform-specific differences, especially for image editing on iOS and Android.
*   **Error Handling:**  The code includes some error handling (e.g., checking permissions), but could be more robust.  Consider adding more specific error handling for image processing and upload failures.
*   **UI/UX:**  The UI could be further improved for better user experience.  Consider adding loading indicators, progress bars, and better feedback for different actions.
*   **Performance:**  Be mindful of image sizes and processing times, especially on lower-end devices.  Optimize image processing and consider using caching mechanisms if necessary.
* **CameraView Ratio:** The `CameraView` component is configured with a `4:3` aspect ratio.  You might need to adjust this depending on your requirements.
* **Image Preview:** The captured image is previewed in the `CameraView` before being confirmed.
* **Flashlight Control:** The component provides a button to toggle the camera's flashlight.
* **Camera Switching:**  The component allows switching between the front and back cameras.
* **Confirmation and Cancellation:** The component provides buttons to confirm or cancel the image capture/selection.
* **Loading Indicator:**  An `ActivityIndicator` is displayed while the camera is taking a picture or while the image is being processed/uploaded.
* **Global State Connection:** The component connects to the global state to manage its visibility and the currently selected image.
* **Navigation:** The component uses `LibNavigation` to navigate to the image cropping screen.
* **Image Upload:** The `processImage` function handles uploading the image to the server using `LibCurl`.
* **Multiple Image Selection:** The `fromGallery` function supports selecting multiple images from the gallery.
* **Image Resizing:** The `processImage` function resizes the image before uploading it.
* **Error Handling:** The code includes some basic error handling, but could be improved to handle more specific error cases.
* **UI/UX:** The UI could be further enhanced with better styling, animations, and feedback to the user.
* **Performance:** Image processing and uploading