# LibNavigation

This module provides a comprehensive set of navigation utilities for React Native applications, built on top of React Navigation. It simplifies navigation, argument passing, result handling, tab management, and more.  It integrates with `esoftplay` for global state management and user data.

## Installation

This module relies on `@react-navigation/native` and `esoftplay`. Ensure you have them installed:

```bash
npm install @react-navigation/native esoftplay
# or
yarn add @react-navigation/native esoftplay
```

## Usage

```javascript
import { LibNavigation } from 'esoftplay/cache/lib/navigation/import';

// Navigate to a screen:
LibNavigation.navigate('main/index');

// Get arguments passed to the current screen:
const userId = LibNavigation.getArgs(props, 'userId');

// Get all arguments passed to the current screen
const allArgs = LibNavigation.getArgsAll(props);

// Handle a result from a navigated screen:
LibNavigation.navigateForResult('user/location', { initialData: locationData }, 1).then(result => {
  // Use the result
});

// Go back:
LibNavigation.back();

// Go back to the root screen:
LibNavigation.backToRoot();

// Reset the navigation stack:
LibNavigation.reset();

// Get the current route name:
const currentRouteName = LibNavigation.getCurrentRouteName();

// Check if the current route is the first route
const isFirstRoute = LibNavigation.isFirstRoute();

// Create and manage tab configurations:
const tabConfig = LibNavigation.createTabConfig(['HomeScreen', 'ProfileScreen']);
const tabConfigState = LibNavigation.useTabConfigState(tabConfig); // Access and update the tab state

// Inject navigation props into a component:
<LibNavigation.Injector args={{ userId: 456 }}>
  <MyComponent />
</LibNavigation.Injector>
```

## API

### Core Navigation

#### `setRef(ref: any): void`
Sets the React Navigation ref. This is crucial for using most of the navigation functions.  Usually, you'll set this in your root navigation container.
#### `setIsReady(isReady: boolean): void`
Sets the navigation readiness.
#### `getIsReady(): boolean`
Gets the navigation readiness.
#### `setNavigation(nav: any): void`
Sets the navigation object.
#### `navigation(): any`
Gets the navigation object.
#### `navigate<S extends keyof EspArgsInterface>(route: S, params?: EspArgsInterface[S]): void`
Navigates to a specified route with optional parameters.
#### `navigateTab<S extends keyof EspArgsInterface>(route: S, tabIndex: number, params?: EspArgsInterface[S]): void`
Navigates to a route within a tab navigator and sets the active tab.
#### `replace<S extends keyof EspArgsInterface>(route: S, params?: EspArgsInterface[S]): void`
Replaces the current screen with a new route and optional parameters.
#### `push<S extends keyof EspArgsInterface>(route: S, params?: EspArgsInterface[S]): void`
Pushes a new screen onto the navigation stack.
#### `reset(route?: LibNavigationRoutes, ...routes: LibNavigationRoutes[]): void`
Resets the navigation stack to a specified route or set of routes.
#### `back(deep?: number): void`
Navigates back a specified number of screens.
#### `backToRoot(): void`
Navigates back to the root screen of the navigation stack.

### Arguments and Results

#### `getArgs(props: any, key: string, defOutput?: any): any`
Retrieves a specific argument passed to the current screen.
#### `getArgsAll<S extends keyof EspArgsInterface>(props: any, defOutput?: any): EspArgsInterface[S]`
Retrieves all arguments passed to the current screen.
#### `useBackResult(props: any): (res: any) => void`
Hook to handle results from a navigated screen.
#### `getResultKey(props: any): number`
Gets the result key for the current screen.
#### `cancelBackResult(key?: number): void`
Cancels a pending back result.
#### `sendBackResult(result: any, key?: number): void`
Sends a result back to the calling screen.
#### `navigateForResult<S extends keyof EspArgsInterface>(route: S, params?: EspArgsInterface[S], key?: number): Promise<any>`
Navigates to a screen and returns a Promise that resolves with the result.

### Tab Navigation

#### `createTabConfig<S extends keyof EspArgsInterface>(modules: S[], defaultIndex?: number): useGlobalReturn<LibNavigationTabConfigReturn<S>>`
Creates a tab configuration object.
#### `useTabConfigState<S extends keyof EspArgsInterface>(tabConfig: useGlobalReturn<LibNavigationTabConfigReturn<S>>)`
Hook to access and manage the tab configuration state.

### Other Utilities

#### `getCurrentRouteName(): string`
Returns the name of the current route.
#### `isFirstRoute(): boolean`
Checks if the current route is the first route in the navigation stack.
#### `Injector(props: LibNavigationInjector): any`
Injects navigation props into a component.
#### `setRedirect(func: Function, key?: number)`
Sets a redirect function.
#### `delRedirect(key?: number)`
Deletes a redirect function.
#### `redirect(key?: number)`
Executes a redirect function.

## Interfaces

*   `LibNavigationTabConfigReturn<S extends keyof EspArgsInterface>`: Interface for tab configuration object.
*   `LibNavigationInjector`: Interface for the `Injector` component's props.

## Important Considerations

*   **React Navigation Integration:** This module is built on top of React Navigation. Make sure you have React Navigation set up correctly in your project.
*   **`esoftplay` Dependency:** This module integrates with `esoftplay` for global state management.
*   **Navigation Ref:** Setting the navigation ref using `setRef` is essential for using most of the navigation functions.
*   **Route and Params:**  Routes and parameters are handled similarly to React Navigation.
*   **Tab Management:** The `createTabConfig` and `useTabConfigState` functions provide a convenient way to manage tab navigation state.
*   **Result Handling:** The `navigateForResult`, `useBackResult`, and related functions simplify handling results from navigated screens.
*   **Redirects:** The `setRedirect`, `delRedirect` and `redirect` functions allow for setting and executing redirect functions.
*   **Current Route:** The `getCurrentRouteName` and `isFirstRoute` functions provide information about the current navigation state.
*   **Injector:** The `Injector` component simplifies passing navigation arguments to child components.
*   **Type Safety:** The module uses generics (`<S extends keyof EspArgsInterface>`) to provide type safety for route names and parameters.  Make sure you have defined the `EspArgsInterface` type in your project.  This interface should define the types of the parameters for each of your routes.
