# UseCurl

This custom hook simplifies making API requests using `LibCurl` from `esoftplay`, managing loading and error states, and optionally displaying a progress indicator.

## Installation

This hook requires `esoftplay`. Ensure it's installed:

```bash
npm install esoftplay
# or
yarn add esoftplay
```

## Usage

```javascript
import { UseCurl } from 'esoftplay/cache/use/curl/import';

function MyComponent() {
  const [makeRequest, loading, error] = UseCurl(false, "Loading data..."); // Initial loading: false, progress text: "Loading data..."

  const fetchData = () => {
    makeRequest('/api/data', { /* request body */ }, (response, message) => {
      if (message === 'success') {
        console.log("API Response:", response);
      } else {
        console.error("API Error:", message);
      }
    });
  };

  return (
    <View>
      <Button title="Fetch Data" onPress={fetchData} disabled={loading} />
      {loading && <ActivityIndicator />}
      {error && <Text style={{ color: 'red' }}>{error}</Text>}
    </View>
  );
}
```

## API

### `UseCurl(initialWithLoading?: boolean, withProgressText?: string): [(uri: string, post: any, onDone: (res: any, msg: string) => void, debug?: 0 | 1) => void, boolean, string]`

Returns an array containing:

1.  `curl`: A function to make API requests: `(uri: string, post: any, onDone: (res: any, msg: string) => void, debug?: 0 | 1) => void`
2.  `loading`: A boolean indicating if a request is in progress.
3.  `error`: A string containing the error message (if any).

## Parameters

*   `initialWithLoading`: (Optional) Initial loading state. Defaults to `false`.
*   `withProgressText`: (Optional) Text for the progress indicator. If omitted, no indicator is shown.

## Functionality

1.  **Loading State Management:** Uses `useSafeState` to manage the `loading` state, set to `true` before requests and `false` after completion (success or error).

2.  **Error Handling:** Uses `useSafeState` to manage the `error` state, storing error messages from API requests.

3.  **`LibCurl` Integration:** Uses `LibCurl` for making API calls.

4.  **Progress Indicator (Optional):** If `withProgressText` is provided, shows a progress indicator using `LibProgress.show` and hides it upon request completion.

5.  **`curl` Function:** The returned `curl` function takes:
    *   `uri`: API endpoint URL.
    *   `post`: Request body (usually an object).
    *   `onDone`: Callback for successful requests, receiving the response and a status message.
    *   `debug`: (Optional) Debug flag (0 or 1).

## Important Considerations

*   **`esoftplay` Dependencies:** Relies on `esoftplay`'s `LibCurl`, `LibProgress`, and `useSafeState`.
*   **Error Handling:** Basic error handling is provided by storing the error message.  Implement more robust error handling in your component.
*   **Loading State:** Use the `loading` state to provide user feedback (e.g., disable buttons, show a loading indicator).
*   **Progress Indicator:** The `withProgressText` parameter allows displaying a progress indicator with custom text.
*   **Customization:**  Customize the hook's behavior by modifying how `LibCurl` is used or adding more error-handling logic.
*   **`useSafeState`:** This custom hook likely handles state updates safely, especially in asynchronous contexts, preventing memory leaks and updates after component unmount.
*   **Asynchronous Operations:** The hook uses callbacks to handle asynchronous API requests.
*   **Simplified API Calls:** The hook simplifies API calls by abstracting `LibCurl` usage and loading/error state management.
*   **Reusability:** The hook is reusable across your application.
*   **Default Values:** `initialWithLoading` and `withProgressText` have default values for easier use.
*   **Debugging:** The `debug` parameter allows debugging `LibCurl`.
*   **Flexibility:** The hook is flexible enough for various API requests and responses. Customize the `onDone` callback for specific response handling.
