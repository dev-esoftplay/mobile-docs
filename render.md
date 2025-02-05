# useRenderCounter Hook Documentation

This document describes the `useRenderCounter` custom React hook.  This hook provides a simple way to display a render counter for a given module, useful for debugging and performance analysis.

## Installation

This is a custom hook that relies on `react-native-reanimated`. Ensure you have the necessary dependencies installed:

```bash
npm install react-native-reanimated
# or
yarn add react-native-reanimated
```

You'll also need to configure Reanimated; refer to the Reanimated documentation for setup instructions.

## Usage

```javascript
import useRenderCounter from './path/to/useRenderCounter'; // Adjust path as needed

function MyComponent() {
  const RenderCounter = useRenderCounter("MyComponent"); // Pass the module name

  return (
    <View>
      {/* ... your component content ... */}
      <RenderCounter /> {/* Include the counter component */}
    </View>
  );
}

```

## API Reference

### `useRenderCounter(module: string): () => JSX.Element`

A custom React hook that returns a function which, when called, renders a component displaying the render count.

* **Parameters:**
    * `module`: A string representing the name of the module or component.  This is used in the displayed counter.
* **Returns:** A function that returns a `JSX.Element` (specifically, an `AnimatedText` component).

## How it Works

The hook uses a `ref` to track the render count.  The `useEffect` hook increments this counter on every render.  It also utilizes `react-native-reanimated` to animate the opacity of the counter, making it appear and fade out briefly after each render.

The returned function creates an `AnimatedText` component that displays the module name and the current render count.  The `useAnimatedStyle` hook connects the opacity of the `AnimatedText` to the `value` shared value, enabling the fade-in/fade-out animation.  The component is positioned absolutely at the bottom-left of the screen with a black background and white text.

## Component

The component returned by the function provided by `useRenderCounter` is an `Animated.createAnimatedComponent(Text)`. It displays the module name and the render count.  It has the following styles applied:

* `position: 'absolute'`
* `fontSize: 10`
* `zIndex: 999`
* `color: 'white'`
* `backgroundColor: 'black'`
* `bottom: 0`
* `left: 0`

It also has `pointerEvent={'none'}` set, preventing it from intercepting touch events.

## Animation

The animation uses `react-native-reanimated`'s `withDelay` and `withTiming` functions to create a fade-in/fade-out effect.  The opacity starts at 0, then animates to 1 with a 1-second delay, and finally animates back to 0 over a 500ms duration.

## Benefits

* **Easy Render Counting:** Provides a simple way to monitor how many times a component renders.
* **Visual Feedback:**  The animated appearance of the counter makes it easy to see when re-renders occur.
* **Debugging Tool:** Useful for identifying unnecessary re-renders and optimizing component performance.

## Example

```javascript
import useRenderCounter from './path/to/useRenderCounter';

function MyComponent() {
  const RenderCounter = useRenderCounter("MyComponent");

  return (
    <View>
      {/* ... component content ... */}
      <RenderCounter />
    </View>
  );
}
```

Every time `MyComponent` renders, the counter at the bottom-left of the screen will briefly show the updated render count (e.g., "MyComponent: 3") with a fade-in/fade-out animation.  This allows you to quickly identify how often the component is re-rendering, aiding in performance analysis.
