# UserRoutes

This module manages the application's navigation routes using a global state object. It provides functions to set the routes and retrieve the current route name.

## Installation

This module relies on `esoftplay`. Ensure it is installed:

```bash
npm install esoftplay
# or
yarn add esoftplay
```

## API

### `state(): useGlobalReturn<any>`

Returns the global state object containing the navigation routes.

### `set(routes: any): void`

Sets the navigation routes in the global state.  The `routes` object is likely an array of route objects, each containing information about a screen or navigation destination.

### `getCurrentRouteName(): string`

Returns the name of the current route.  It retrieves the last route from the `routes` array in the global state.  If no routes are available, it returns 'root'.

## Functionality

1.  **State Management:** Uses `useGlobalState` to store and manage the navigation routes in a global state object.

2.  **Route Setting:** The `set` function updates the navigation routes in the global state.

3.  **Current Route Retrieval:** The `getCurrentRouteName` function retrieves the name of the current route from the global state.  It accesses the last element of the `routes` array and returns its `name` property.

## Important Considerations

*   **`esoftplay` Dependencies:** Relies on `esoftplay`'s global state management (`useGlobalState`).
*   **Global State:** Uses a global state object to store the routes.  Be mindful of the implications of using global state, especially in larger applications.
*   **Route Object Structure:** The structure of the `routes` object is not explicitly defined, but it's assumed to be an array of route objects, each with a `name` property.
*   **Default Route Name:** If no routes are available, the `getCurrentRouteName` function returns 'root'.
*   **Navigation Library Integration:** This module is likely used in conjunction with a navigation library (e.g., React Navigation).  The `routes` object is probably populated by the navigation library.
*   **Route Name Consistency:**  Ensure that the route names used in this module are consistent with the route names defined in your navigation configuration.
*   **Global State Considerations:** While using global state for navigation routes can be convenient, it might not be the most scalable solution for large applications.  Consider using a more structured state management approach or context if your navigation structure becomes complex.
* **Route Object Structure:**  While not explicitly defined, the `routes` object is likely an array of route objects. Each route object probably has a `name` property (used by `getCurrentRouteName`) and might have other properties like `params`, `path`, or `component`.  Understanding the structure of these route objects is important for working with this module.
* **Navigation Integration:** This module likely integrates with a navigation library like React Navigation.  The `routes` array is probably populated and managed by the navigation library.  This module provides a way to access the current route information from anywhere in the application.
* **Error Handling (Implicit):**  The `getCurrentRouteName` function handles the case where the `routes` array is empty or `null` by returning 'root'.  This prevents potential errors when trying to access the last element of the array.
* **Global State Usage:** The use of `useGlobalState` is a key aspect of this module.  It makes the navigation state available throughout the application.  However, be aware of the potential drawbacks of global state, especially in larger applications.  Consider using a more structured state management solution if necessary.
* **Route Name Convention:**  The default route name 'root' is a common convention.  You can customize this if needed.
* **Potential Use Cases:** This module could be used for:
    *   Conditional rendering based on the current route.
    *   Analytics tracking of screen views.
    *   Managing navigation state in a centralized location.
    *   Deep linking integration.