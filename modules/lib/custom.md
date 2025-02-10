# LibCustom

This component fetches a JSON schema from a URL and renders it using `LibCompose`. It handles both HTTP URLs and deep links, navigating back if a deep link is encountered.

## Installation

This component depends on `esoftplay`. Ensure you have it installed in your project.

```bash
npm install esoftplay
# or
yarn add esoftplay
```

## Usage

```javascript
import LibCustom from './your-lib-custom-file'; // Replace with the correct path

// In your navigation or wherever you're navigating to this component:
esp.mod("lib/navigation").navigate("YourLibCustomRouteName", {
  url: "[https://api.test.bbo.co.id/main?module=lib.custom&link=https%3A%2F%2Ftest.bbo.co.id%2Fsaya-tidak-bisa-login-akun-bbo_432.htm](https://api.test.bbo.co.id/main?module=lib.custom&link=https%3A%2F%2Ftest.bbo.co.id%2Fsaya-tidak-bisa-login-akun-bbo_432.htm)",
  title: "Custom View" // Optional title
});

// Or for a deep link:
esp.mod("lib/navigation").navigate("YourLibCustomRouteName", {
  url: "bbt://test.bbo.co.id/jual/arcanealley2024",
  title: "Deep Link View" // Optional title
});

// Define your route (e.g., in React Navigation):
<Stack.Screen name="YourLibCustomRouteName" component={LibCustom} />
```

## Props

This component doesn't receive props directly. Instead, the `url` and `title` (optional) are passed via navigation arguments using `esp.mod("lib/navigation").navigate`.

## Functionality

1. **Navigation Arguments:** The component retrieves the `url` from the navigation arguments using `esp.mod("lib/navigation").getArgsAll(props)`.

2. **Deep Link Handling:** If the `url` does *not* start with "http", it's treated as a deep link. The component attempts to open the URL using `Linking.openURL` and then navigates back using `esp.mod("lib/navigation").back()`.

3. **HTTP URL Handling:** If the `url` *does* start with "http", it's treated as a URL for a JSON schema.  The component fetches the schema using `esp.mod("lib/curl")`.

4. **Schema Rendering:** Upon successful retrieval of the schema, the component renders it using `LibCompose` (from `esoftplay`).

5. **Error Handling:** If the `curl` request fails, a toast message is displayed using `esp.modProp("lib/toast").show(err?.message)`, and the component navigates back.

6. **Conditional Rendering:** The component renders the `LibCompose` component only if the `url` is an HTTP URL *and* the schema has been successfully fetched. Otherwise, it returns `null`.

## Important Considerations

* **`esoftplay` Dependency:** This component is tightly coupled to the `esoftplay` library.  Make sure it is correctly set up in your project. All modules (navigation, curl, toast, compose) are accessed via `esp`.
* **Navigation:** The component relies on `esp.mod("lib/navigation")` for navigation and passing arguments.  Make sure your navigation setup is compatible with this.
* **Deep Links:** Ensure your deep link scheme (`bbt://` in the example) is correctly configured in your app.
* **Error Handling:**  The error handling is basic (showing a toast and navigating back).  You might want to add more robust error handling, such as displaying a specific error screen.
* **Conditional Rendering:**  The conditional rendering ensures that `LibCompose` is only rendered when the schema is available. This prevents errors and improves performance.
* **`LibCompose`:**  This component relies heavily on `LibCompose` to render the JSON schema.  Make sure you understand how `LibCompose` works and how to define your JSON schemas.
* **useEffect Hook:** The `useEffect` hook with an empty dependency array ensures that the data fetching and deep link handling logic runs only once when the component mounts.
* **useRef Hook:** The `LibCompose` module is accessed using a ref, which is the standard way to interact with modules from `esoftplay`.