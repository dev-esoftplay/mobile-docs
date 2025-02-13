# UseCondition

This component conditionally renders content based on a boolean prop.  It provides a concise way to handle conditional rendering in JSX.

## Installation

This component does not have any external dependencies beyond React Native itself.

## Usage

```javascript
import { UseCondition } from 'esoftplay/cache/use/condition/import';

<UseCondition if={true} children={<Text>Content is true</Text>} fallback={<Text>Content is false</Text>} />

<UseCondition if={false}>
  <Text>This will not be rendered.</Text>
</UseCondition>

<UseCondition if={someVariable}>
  <MyComponent />
</UseCondition>

// Using null as fallback:
<UseCondition if={condition} children={<Text>Content is true</Text>} />

// Using a fragment as children:
<UseCondition if={isLoggedIn}>
  <>
    <Text>Welcome, user!</Text>
    <Button title="Logout" onPress={handleLogout} />
  </>
</UseCondition>
```

## Props

| Prop       | Type    | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| ---------- | ------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `if`       | `boolean` | Yes      | The boolean value that determines whether to render the `children` or the `fallback`.                                                                                                                                                                                                                                                                                                                                                       |
| `children` | `any`   | Yes      | The content to render if the `if` prop is `true`.                                                                                                                                                                                                                                                                                                                                                                                     |
| `fallback` | `any`   | No       | The content to render if the `if` prop is `false`. If not provided, `null` will be rendered.                                                                                                                                                                                                                                                                                                                                                       |

## Functionality

The component uses a ternary operator (`condition ? trueExpression : falseExpression`) to conditionally render content. If the `if` prop is `true`, the `children` prop is rendered. Otherwise, the `fallback` prop is rendered (or `null` if `fallback` is not provided).

## Important Considerations

*   **Conditional Rendering:** This component provides a convenient way to perform conditional rendering in JSX. It simplifies the syntax compared to using ternary operators directly within the JSX.

*   **`children` Prop:** The `children` prop can be any valid JSX, including a single element, an array of elements, or a React Fragment.

*   **`fallback` Prop:** The `fallback` prop is optional. If not provided, the component will render `null` when the `if` prop is `false`.

*   **Boolean Conversion:** The component uses `Boolean(props.if)` to ensure that the `if` prop is treated as a boolean value. This handles cases where the `if` prop might be a truthy or falsy value (e.g., a number or a string).

*   **Conciseness:** This component makes conditional rendering more concise and readable, especially when dealing with more complex conditions or content.

*   **Flexibility:** The `children` and `fallback` props can accept any valid JSX, allowing for a wide range of conditional rendering scenarios.

* **Null Fallback:**  When the `fallback` prop is not provided and the `if` condition is false, the component renders `null`. This is a common pattern in React to avoid rendering anything when a condition is not met.  It's important to be aware of this behavior, especially if you're expecting something else to be rendered in the fallback case.
* **Boolean Type Handling:**  The `Boolean(props.if)` ensures that the `if` prop is always treated as a boolean. This is important because JavaScript is loosely typed, and values like 0, an empty string, or `undefined` can be coerced to `false` in conditional contexts.  By explicitly converting the `if` prop to a boolean, you avoid unexpected behavior.
* **JSX Flexibility:** The `children` and `fallback` props can accept any valid JSX.  This includes single elements, arrays of elements, or even React Fragments.  This makes the `UseCondition` component very flexible and reusable.  You can use it to conditionally render anything from a simple text element to a complex component structure.
