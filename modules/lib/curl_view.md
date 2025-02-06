# LibCurl_view

This component fetches data from a URL using `LibCurl` and renders the result based on success, error, or loading states. It supports caching, user data registration, and custom rendering for each state.

## Installation

This component relies on the `esoftplay` library. Ensure you have it installed in your project.

```bash
npm install esoftplay
# or
yarn add esoftplay
```

## Usage

```javascript
import { LibCurl_view } from 'esoftplay/cache/lib/curl_view/import';

<LibCurl_view
  url="https://api.example.com/data"
  post={{ /* optional post data */ }}
  cache={true} // Optional: Enables caching
  isUserData={true} // Optional: Registers data with UserData
  onSuccess={(res, message) => ( // Required: Render on success
    <View>
      <Text>Data: {JSON.stringify(res)}</Text>
      <Text>Message: {message}</Text>
    </View>
  )}
  onError={(err, retry) => ( // Required: Render on error
    <View>
      <Text>Error: {err.message}</Text>
      <Button title="Retry" onPress={retry} />
    </View>
  )}
  onLoading={<ActivityIndicator />} // Optional: Render while loading
/>
```

## Props

| Prop        | Type                  | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| ----------- | --------------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `url`       | `string`              | Yes      | The URL to fetch data from.                                                                                                                                                                                                                                                                                                                                                                                    |
| `post`      | `any`                 | No       | Optional data to be sent in a POST request.                                                                                                                                                                                                                                                                                                                                                                |
| `cache`     | `boolean`             | No       | Whether to cache the response.                                                                                                                                                                                                                                                                                                                                                                                  |
| `isUserData` | `boolean`             | No       | Whether to register the cached data with `UserData` (from `esoftplay`).                                                                                                                                                                                                                                                                                                                                          |
| `onSuccess` | `(res: any, message: string) => ReactElement` | Yes      | Function to render the content when the request is successful. Receives the response data (`res`) and message (`message`) as arguments. Should return a React element.                                                                                                                                                                                                                          |
| `onError`   | `(err: any, retry: Function) => ReactElement` | Yes      | Function to render the content when the request fails. Receives the error object (`err`) and a `retry` function as arguments. The `retry` function can be called to retry the request. Should return a React element.                                                                                                                                                              |
| `onLoading` | `ReactElement`        | No       | React element to render while the request is in progress. If not provided, a default loading indicator (`LibLoading` from `esoftplay`) is used.                                                                                                                                                                                                                                                        |

## Functionality

1. **Data Fetching:** The component uses `LibCurl` (from `esoftplay`) to make the network request.

2. **Caching:** If the `cache` prop is `true`, the response is stored using `FastStorage` (likely `react-native-mmkv` wrapped by `esoftplay`). The cache key is generated from the URL and POST data.

3. **User Data Registration:** If `cache` and `isUserData` are both `true`, the cached data's key is registered with `UserData` (from `esoftplay`). This could be for persisting user-specific data.

4. **State Management:** `useSafeState` (from `esoftplay`) manages the component's state, holding the fetched data or error information.

5. **Rendering:**
   - **Loading:** If no data is available (initially or during a retry), the `onLoading` prop is rendered. If `onLoading` isn't provided, `LibLoading` (from `esoftplay`) is displayed.
   - **Success:** If the request is successful, the `onSuccess` prop is called with the response data and message.
   - **Error:** If the request fails, the `onError` prop is called with the error object and a `retry` function.

6. **Retry Mechanism:** The `onError` prop receives a `retry` function, which, when called, resets the data state and triggers the `fetchData` function again, effectively retrying the request.

## `onSuccess` and `onError` Functions

The `onSuccess` and `onError` props are functions that allow you to customize the rendering based on the request outcome.  They *must* return a React element.

* **`onSuccess(res, message)`:**
    * `res`: The parsed JSON response from the API.
    * `message`: The message associated with the response (if any).

* **`onError(err, retry)`:**
    * `err`: The error object.  This will likely have a `message` property.
    * `retry`: A function that you can call to retry the API request.  This is crucial for providing a user-friendly way to handle network issues.

## `onLoading` Prop

The `onLoading` prop lets you specify what to display while the data is being fetched.  If you don't provide this prop, the component will use a default loading indicator (`LibLoading` from `esoftplay`).

## Important Considerations

* **`esoftplay` Dependency:** This component is tightly coupled to the `esoftplay` library.  Ensure you have `esoftplay` set up correctly in your project.  All the imports should come directly from `esoftplay` (e.g., `import { LibCurl } from 'esoftplay';`).
* **Error Handling:** The `onError` prop is vital for handling network errors gracefully.  Make sure to implement it to provide informative error messages and a way for the user to retry the request.
* **Caching:**  Caching can significantly improve performance, but be mindful of cache invalidation strategies.  If the data changes frequently, you might need a mechanism to refresh the cache.
* **`isUserData`:**  The `isUserData` prop is specific to the `esoftplay` framework and its `UserData` module.  If you're not using `UserData`, you can set this to `false`.
* **Type Safety:**  Consider using TypeScript or Flow to add type safety to your props and ensure that the `onSuccess` and `onError` functions return the correct React elements.
