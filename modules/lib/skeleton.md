# LibSkeleton

This component provides a skeleton loading effect, which is a common way to indicate that content is loading. It uses a masked view with a shimmering gradient animation to create the skeleton effect.  It also provides convenient static methods for creating different shapes of skeleton elements.

## Installation

This component relies on `@react-native-masked-view/masked-view`, `expo-linear-gradient` and `react-native-reanimated`. Ensure you have them installed:

```bash
npm install @react-native-masked-view/masked-view expo-linear-gradient react-native-reanimated
# or
yarn add @react-native-masked-view/masked-view expo-linear-gradient react-native-reanimated
```

You'll also need to configure `react-native-reanimated` if you haven't already.  Follow the instructions in the library's documentation.

## Usage

```javascript
import { LibSkeleton } from 'esoftplay/cache/lib/skeleton/import';

// Basic skeleton:
<LibSkeleton />

// With custom duration and reverse animation:
<LibSkeleton duration={500} reverse={true} />

// With custom colors:
<LibSkeleton colors={['#e0e0e0', '#f0f0f0', '#e0e0e0']} />

// With custom background style:
<LibSkeleton backgroundStyle={{ backgroundColor: 'white' }} />

// With children (for custom skeleton shapes):
<LibSkeleton>
  <LibSkeleton.BoxFull size={20} />
  <LibSkeleton.BoxHalf size={15} />
  <LibSkeleton.Circle size={30} />
</LibSkeleton>

// Using the static methods to create skeleton elements directly:
<LibSkeleton>
  <LibSkeleton.BoxFull size={20} />
  <LibSkeleton.BoxFlex size={15} />
  <LibSkeleton.Box size={10} />
  <LibSkeleton.Circle size={25} />
</LibSkeleton>
```

## API

### `LibSkeleton` (Component)

Renders the skeleton effect.

### Static Methods

*   `LibSkeleton.BoxFull(props: LibSkeletonStatic): any`: Creates a full-width rectangular skeleton box.
*   `LibSkeleton.BoxFlex(props: LibSkeletonStatic): any`: Creates a flexible-width rectangular skeleton box that expands to fill available space.
*   `LibSkeleton.BoxHalf(props: LibSkeletonStatic): any`: Creates a half-width rectangular skeleton box.
*   `LibSkeleton.Box(props: LibSkeletonStatic): any`: Creates a square skeleton box.
*   `LibSkeleton.Circle(props: LibSkeletonStatic): any`: Creates a circular skeleton element.

## Interfaces

### `LibSkeletonProps`

```typescript
interface LibSkeletonProps {
  duration?: number
  reverse?: boolean,
  colors?: string[],
  backgroundStyle?: any,
  children?: any
}
```

*   `duration`: The duration of the shimmer animation in milliseconds. Defaults to 1000.
*   `reverse`: Whether the shimmer animation should play in reverse. Defaults to `false`.
*   `colors`: An array of colors to use for the gradient. Defaults to a light gray shimmer.
*   `backgroundStyle`: Style for the background of the skeleton.
*   `children`:  Optional.  If provided, these elements will be used as the mask.  This is useful for creating custom skeleton shapes.

### `LibSkeletonStatic`

```typescript
interface LibSkeletonStatic {
  size: number
}
```

*   `size`: The size (height, width, diameter) of the skeleton element.

## Functionality

1. **Masked View:** The component uses `MaskedView` to create the skeleton effect. The `children` prop (or a default set of rectangles if no children are provided) acts as the mask.

2. **Shimmering Gradient:** A `LinearGradient` is animated across the masked view to create the shimmering effect.

3. **Animation:** The `react-native-reanimated` library is used to create the smooth, repeating animation of the gradient.

4. **Custom Shapes:** The static methods (`BoxFull`, `BoxFlex`, `BoxHalf`, `Box`, `Circle`) provide convenient ways to create common skeleton shapes. You can also pass custom elements as children to the `LibSkeleton` component to create more complex skeleton layouts.

## Important Considerations

*   **Dependencies:** Make sure you have all the required libraries installed and configured.
*   **Custom Shapes:** The `children` prop provides great flexibility for creating custom skeleton layouts.
*   **Animation:** The `duration` and `reverse` props allow you to customize the shimmer animation.
*   **Color Customization:** The `colors` prop allows you to change the colors of the shimmering gradient.
*   **Performance:** The use of `react-native-reanimated` should provide good performance for the animation.  However, for very complex skeleton layouts, you might need to consider further optimizations.
* **Accessibility:** While this component provides a visual skeleton, it's essential to consider accessibility.  When the actual content loads, ensure that screen readers and other assistive technologies can properly access and interpret the content.  You might want to use ARIA attributes to indicate the loading state while the skeleton is visible.
* **Static Methods:** The static methods are a convenient way to create common skeleton shapes.  They simplify the process of building skeleton layouts.
* **Flexibility:**  The combination of the static methods and the `children` prop gives you a lot of flexibility in creating different skeleton layouts. You can create very complex and realistic skeleton screens to improve the user experience during loading.
