# LibComponent

This is a base component class designed to extend React's built-in `Component` class. It provides several enhancements, including safe `setState` calls, optimized `shouldComponentUpdate`, and a stub `onBackPress` method.

## Installation

This component doesn't require any external dependencies beyond what's included in React. Just include the code in your project.  It uses `react-fast-compare` so you need to install it.

```bash
npm install react-fast-compare
# or
yarn add react-fast-compare
```

## Usage

Extend your components from `LibComponent` instead of `React.Component`:

```javascript
import { LibComponent } from 'esoftplay/cache/lib/component/import';

interface MyComponentProps {
  // ... your props
  name: string;
}

interface MyComponentState {
  // ... your state
  count: number;
}

class MyComponent extends LibComponent<MyComponentProps, MyComponentState> {
  constructor(props: MyComponentProps) {
    super(props);
    this.state = {
      count: 0,
    };
  }

  render() {
    return (
      <div>
        <p>Name: {this.props.name}</p>
        <p>Count: {this.state.count}</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>Increment</button>
      </div>
    );
  }
}

export default MyComponent;
```

## Key Features

### 1. Safe `setState`

The `setState` method is overridden to prevent setting state after the component has unmounted.  This avoids potential memory leaks and errors.  It checks the `_isMounted` flag before calling `super.setState`.

### 2. Optimized `shouldComponentUpdate`

The `shouldComponentUpdate` method is implemented using `react-fast-compare`'s `isEqual` function. This provides a more efficient way to compare props and state for changes, potentially improving performance by avoiding unnecessary re-renders.

### 3. Stub `onBackPress` Method

An `onBackPress` method is provided as a stub. This method can be overridden in child classes to handle back press events (e.g., in React Navigation).  It currently returns `true` by default.

## API

### `setState(state, callback)`

Sets the component's state.  This is the overridden version that ensures the component is still mounted before setting the state.

* **Parameters:**
    * `state`: The new state value or a function that returns the new state.  Same as the standard `React.Component.setState` parameters.
    * `callback` (optional): A function that will be called after the state has been updated.

* **Returns:** `void`

### `shouldComponentUpdate(nextProps, nextState)`

Determines if the component should re-render.  Uses `react-fast-compare` for efficient comparison of props and state.

* **Parameters:**
    * `nextProps`: The next props.
    * `nextState`: The next state.

* **Returns:** `boolean` - `true` if the component should update, `false` otherwise.

### `onBackPress()`

A stub method for handling back press events.

* **Returns:** `boolean` - `true` to allow default back press behavior, `false` to prevent it.

## Example: Overriding `onBackPress`

```javascript
import { LibComponent } from 'esoftplay/cache/lib/component/import';

class MyComponent extends LibComponent<MyComponentProps, MyComponentState> {
  onBackPress(): boolean {
    // Custom logic for handling back press
    console.log("Back button pressed!");
    return false; // Prevent default back behavior
  }

  // ... rest of your component
}
```

## Important Considerations

* **Generics:** The class uses generics `<K, S>` for type safety.  `K` represents the type of the props, and `S` represents the type of the state.  Make sure to define these interfaces or types when using `LibComponent`.
* **`react-fast-compare`:** This library is used for shallow comparison of props and state.  It's generally more performant than a deep comparison, but it might not be suitable for all situations. If you have deeply nested objects in your props or state, you might need to consider a deep comparison library or implement a custom `shouldComponentUpdate` method.
* **`onBackPress` Stub:** The `onBackPress` method is a stub.  You must override it in your child components if you need to handle back press events.  It's especially relevant in navigation contexts.
* **`_isMounted` Flag:**  The `_isMounted` flag is crucial for preventing `setState` calls after a component has unmounted. This helps avoid memory leaks and potential errors.  Don't rely on it within asynchronous callbacks that might execute *after* the component has unmounted; use the `isMounted()` lifecycle method for that purpose.
