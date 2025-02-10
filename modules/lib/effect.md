# LibEffect

This component provides a way to conditionally re-render its children based on changes to a dependency array, similar to React's `useEffect` hook but without the side effects. It's useful for optimizing re-renders when only specific parts of the UI depend on certain values.

## Installation

This component relies on `esoftplay` and `react-fast-compare`. Ensure you have them installed:

```bash
npm install esoftplay react-fast-compare
# or
yarn add esoftplay react-fast-compare
```

## Usage

```javascript
import { LibEffect } from 'esoftplay/cache/lib/effect/import';

// In your component:
<LibEffect deps={[data1, data2]}>
  {/* This content will only re-render if data1 or data2 changes */}
  <MyComponentThatUsesData1AndData2 data1={data1} data2={data2} />
</LibEffect>

<LibEffect deps={[otherData]}>
  {/* This content will only re-render if otherData changes */}
  <MyOtherComponent otherData={otherData} />
</LibEffect>

// No deps provided, will always re-render when the parent re-renders
<LibEffect>
  <MyComponentAlwaysReRender />
</LibEffect>

```

## Props

| Prop      | Type    | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| --------- | ------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `deps`    | `any[]` | No       | An array of dependencies.  The component will only re-render if this array changes (using `react-fast-compare` for comparison). If not provided, the component will re-render whenever its parent re-renders.                                                                                                                                                                                           |
| `children` | `any`   | No       | The content to be rendered. This can be a single React element or a function that returns a React element. If children is a function, it doesn't receive any arguments.                                                                                                                                                                                                                                                                                                                       |

## Functionality

1. **Dependency Tracking:** The component tracks the `deps` prop.  It initializes `this.deps` in `componentDidMount`.

2. **`shouldComponentUpdate`:** The `shouldComponentUpdate` method is overridden to compare the new `deps` prop with the previous one using `react-fast-compare`'s `isEqual` function. If the dependencies are different, the component re-renders. If `deps` is not provided, it defaults to always re-rendering.

3. **Rendering:** The component renders its `children`.

## Important Considerations

* **`esoftplay` Dependency:** This component extends `LibComponent` from `esoftplay`. Make sure `esoftplay` is correctly set up in your project.
* **`react-fast-compare`:** The `isEqual` function from `react-fast-compare` is used for efficient shallow comparison of the dependency arrays.  This is important for performance.
* **Shallow Comparison:** `react-fast-compare` performs a shallow comparison.  If your dependencies are deeply nested objects, changes within those objects might not trigger a re-render.  In such cases, you might need to use a deep comparison library or restructure your data.
* **No Side Effects:** Unlike React's `useEffect`, this component does *not* perform any side effects. It's purely for controlling re-renders based on dependencies. It's a performance optimization tool.
* **Children:**  The `children` prop can be a React element or a function that returns a React element.  This allows for flexibility in how the content is rendered.
* **When to use:**  Use this component when you have parts of your UI that depend on specific values and you want to prevent unnecessary re-renders of those parts when other, unrelated parts of the parent component re-render.  It's a micro-optimization tool that can be helpful in complex components.
* **Alternatives:**  Consider using React's `useMemo` for similar memoization of values. `LibEffect` is more specifically for conditionally rendering parts of the UI, while `useMemo` is for memoizing computed values.  Often, they can be used together.  For example, you might use `useMemo` to compute a value and then use `LibEffect` to only re-render a component when that memoized value changes.
* **Accessibility:** Consider accessibility implications. If the content changes based on the dependencies, ensure that users are notified of these changes (e.g., using ARIA attributes).
