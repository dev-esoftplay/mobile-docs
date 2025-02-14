# LibCurl

This class provides a wrapper around the `fetch` API for making HTTP requests. It handles setting headers, managing timeouts, handling responses, and reporting errors. It also supports secure requests with API keys and file uploads.

## Installation

This module relies on `esoftplay`. Ensure it's installed:

```bash
npm install esoftplay
# or
yarn add esoftplay
```

## Usage

```javascript
import { LibCurl } from 'esoftplay/cache/lib/curl/import'; // Replace with the correct path

const curl = new LibCurl();


// example GET
curl('/api/endpoint', null, (res) => {
  console.log('Success:', res);
}, (err, timeout) => {
  console.error('Error:', err, 'Timeout:', timeout);
}, 1); // 1 is debug mode

// example POST
curl('/api/endpoint', { data: 'some data' }, (res) => {
  console.log('Success:', res);
}, (err, timeout) => {
  console.error('Error:', err, 'Timeout:', timeout);
});

// example custom
curl().custom('/api/endpoint', { data: 'some data' }, (res) => {
  console.log('Success:', res);
}, (err, timeout) => {
  console.error('Error:', err, 'Timeout:', timeout);
});

// For secure requests:
secure('get_token')()('checkout', {data:"post data"},(res, msg) => {
  // ...
},(msg) => {
  // ...
}, 1)

// For file uploads:
curl().upload('/api/upload', 'file', 'file:///path/to/file', 'image/jpeg', (res) => {
  // ...
});
```

## API

### `LibCurl` (Class)

The class provides methods for making HTTP requests.

### `constructor(uri?: string, post?: any, onDone?: (res: any, msg: string) => void, onFailed?: (error: any, timeout: boolean) => void, debug?: number)`

Constructor for the `LibCurl` class.

*   `uri`: The URI for the request (relative to the base URL).
*   `post`: The data to send in the request body.
*   `onDone`: Callback function for successful requests.
*   `onFailed`: Callback function for failed requests.
*   `debug`: Debug level (0 or 1).

### `custom(uri: string, post?: any, onDone?: (res: any, timeout: boolean) => void, debug?: number): Promise<void>`

Makes a custom HTTP request (GET or POST).

### `secure(token_uri?: string): (apiKey?: string) => (uri: string, post?: any, onDone?: (res: any, msg: string) => void, onFailed?: (error: any, timeout: boolean) => void, debug?: number) => void`

Returns a function to make secure HTTP requests with an API key.

### `withHeader(header: any): (uri: string, post?: any, onDone?: (res: any, msg: string) => void, onFailed?: (error: any, timeout: boolean) => void, debug?: number) => void`

Returns a function to make requests with custom headers.

### `upload(uri: string, postKey: string, fileUri: string, mimeType: string, onDone?: (res: any, msg: string) => void, onFailed?: (error: any, timeout: boolean) => void, debug?: number): void`

Uploads a file.

### `initTimeout(customTimeout?: number): void`

Sets up a timeout for the request.

### `cancelTimeout(): void`

Cancels the request timeout.

### `onFetchFailed(message: string): void`

Callback for fetch failures.

### `buildUri(uri: string): string`

Builds the full URI.

### `setUrl(url: string): void`

Sets the base URL.

### `setUri(uri: string): void`

Sets the URI.

### `setApiKey(apiKey: string): void`

Sets the API key.

### `setHeader(): Promise<void>`

Sets headers for the request.

### `closeConnection(): void`

Aborts the request.

### `onDone(result: any, msg?: string): void`

Callback for successful requests.

### `onFailed(error: any, timeout: boolean): void`

Callback for failed requests.

### `onStatusCode(ok: number, status_code: number, message: string, result: any): boolean`

Handles status codes.

### `urlEncode(str: string): string`

Encodes a string for URL parameters.

### `encodeGetValue(_get: string): string`

Encodes GET request parameters.

### `signatureBuild(): string`

Builds a signature for requests.

### `init(uri: string, post?: any, onDone?: (res: any, msg: string) => void, onFailed?: (error: any, timeout: boolean) => void, debug?: number, upload?: boolean): Promise<void>`

Initializes and makes the HTTP request.

### `onFetched(resText: string | Object, onDone?: (res: any, msg: string) => void, onFailed?: (error: any, timeout: boolean) => void, debug?: number): void`

Handles the fetched response.

### `refineErrorMessage(resText: string): string`

Refines error messages.

### `onError(msg: string): void`

Handles errors.

### `getTimeByTimeZone(GMT: number): number`

Gets the time by time zone.

## Important Considerations

*   **Error Reporting:** Uses `reportApiError` to report API errors.
*   **Configuration:** Reads configuration values from `esp.config()`.
*   **Localization:** Uses `esp.lang` for localized strings.
*   **Timeout:** Implements a timeout mechanism for requests.
*   **Security:** Supports secure requests with API keys.
*   **File Uploads:** Supports file uploads using `FormData`.
*   **CORS:** Sets the `mode` to `cors` for cross-origin requests.
*   **Caching:** Disables caching for requests.
*   **Retry Mechanism:** The commented-out retry mechanism could be implemented for more robust error handling.
*   **Debugging:** Offers a debug option to log requests and responses.
*   **Content-Type:** Sets the `Content-Type` header appropriately for different request types (form data or URL-encoded).
*   **Data Transformation:**  The code includes some logic to parse JSON responses and handle different response structures.  This might need to be adapted depending on your API.
*   **Global State:** The code interacts with global state (e.g., for network status).  Be mindful of the implications of using global state.
*   **Dependencies:** Relies on `esoftplay` for various utilities.
*   **`AbortController`:** Uses `AbortController` to cancel requests.
*   **`FormData`:** Uses `FormData` for file uploads.
*   **`encodeURIComponent` and `decodeURIComponent`:** Used for encoding and decoding URL parameters.  Be sure that the encoding/decoding is handled consistently on both the client and server sides.
*   **Error Handling:**  The error handling could be improved to handle different types of errors more specifically (e.g., network errors, server errors, parsing errors).  The `onFetchFailed` and `onError` functions provide a starting point for this.
*   **Security:**  For secure requests, the code uses an API key.  **Ensure that API keys are managed securely and are not exposed in the client-side code.** Consider using more robust authentication and authorization mechanisms if necessary.
*   **File Uploads:**  The `upload` function handles file uploads using `FormData`.  Make sure that the server is configured to handle multipart form data.
*   **Retry Mechanism:** The commented-out retry logic could be implemented to handle transient network errors.  Consider adding a retry count and exponential backoff to avoid overwhelming the server.
* **Timeout:**  The `initTimeout` and `cancelTimeout` functions provide a way to manage request timeouts.  This is important to prevent requests from hanging indefinitely.  The timeout value can be customized.
* **CORS:**  The `mode: "cors"` option is set for requests.  This is necessary for making cross-origin requests.  Make sure that the server is configured to allow requests from your application's origin.
* **Caching:**  The code disables caching for requests.  This is important for ensuring that you are always getting the latest data.
* **Debugging:**  The `debug` option allows you to log requests and responses.  This can be helpful for debugging API issues.
* **Content-Type:**  The `Content-Type` header is set appropriately for different request types.  This is important for the server to correctly parse the request body.
* **Data Transformation:** The code includes some logic to parse JSON responses.  This might need to be adapted depending on your API.
* **Global State:**  The code interacts with global state (e.g., for network status).  Be mindful of the implications of using global state.
* **Dependencies:**  The code relies on `esoftplay` for various utilities.
* **`AbortController`:**  The `AbortController` is used to cancel requests.  This is important for handling timeouts and user navigation changes.
* **`FormData`:**  The `FormData` object is used for file uploads.
* **`encodeURIComponent` and `decodeURIComponent`:**  These functions are used for encoding and decoding URL parameters.  Make sure that the encoding/decoding is handled consistently on both the client and server sides.