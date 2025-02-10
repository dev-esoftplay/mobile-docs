# LibFadescroll

This component creates a fading scroll effect using `react-native-reanimated`.  It makes a view fade in or out based on the value of a shared scroll value.

## Installation

This component relies on `react-native-reanimated` and `esoftplay`. Ensure you have them installed:

```bash
npm install react-native-reanimated esoftplay
# or
yarn add react-native-reanimated esoftplay
```

## Usage

```javascript
import { LibFadescroll } from 'esoftplay/cache/lib/fadescroll/import';
import Animated, { useSharedValue } from 'react-native-reanimated';
import { ScrollView } from 'react-native';

function MyComponent() {
  const scrollController = useSharedValue(0);

  return (
    <ScrollView
      onScroll={Animated.event([{ nativeEvent: { contentOffset: { y: scrollController } } }], { useNativeDriver: true })}
      scrollEventThrottle={16} // Adjust as needed
    >
      <LibFadescroll
        scrollController={scrollController}
        interpolateScroll={[0, 100]} // Scroll values for interpolation
        interpolateOpacity={[0, 1]} // Opacity values for interpolation
        style={{ height: 200, backgroundColor: 'lightblue' }} // Style for the fading view
      >
        {/* Content that will fade in/out */}
        <Text>Scroll down to see the fade effect.</Text>
      </LibFadescroll>
      {/* Rest of your scroll view content */}
      <View style={{ height: 500, backgroundColor: 'lightgray'}}/>
    </ScrollView>
  );
}
```

## Props

| Prop              | Type                | Required | Description                                                                                                                                                                                                                                                                                                                                                                                    |
| ----------------- | ------------------- | -------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `scrollController` | `SharedValue<number>` | Yes      | A shared value (from `react-native-reanimated`) that represents the scroll offset.  This is typically connected to the `onScroll` event of a `ScrollView`.                                                                                                                                                                                                                                                             |
| `style`           | `ViewStyle`         | No       | Style for the `Animated.View` that contains the fading content.                                                                                                                                                                                                                                                                                                                                                         |
| `children`        | `ReactElement`      | No       | The content that will fade in or out.                                                                                                                                                                                                                                                                                                                                                                              |
| `interpolateOpacity` | `number[]`        | Yes      | An array of opacity values corresponding to the `interpolateScroll` values.  These define the opacity at different scroll positions.  Should have the same length as `interpolateScroll`.                                                                                                                                                                                                                          |
| `interpolateScroll` | `number[]`        | Yes      | An array of scroll offset values. These define the scroll positions at which the opacity should change.  Should have the same length as `interpolateOpacity`. The values should be in ascending order.                                                                                                                                                                                                                          |

## Functionality

1. **Shared Value:** The `scrollController` prop is a `SharedValue` from `react-native-reanimated`, which is used to track the scroll offset.

2. **Interpolation:** The `interpolate` function from `react-native-reanimated` is used to map the `scrollController` value to an opacity value. The `interpolateScroll` and `interpolateOpacity` props define the input and output ranges for this mapping. `Extrapolation.CLAMP` is used to prevent opacity values from going outside the 0-1 range.

3. **Animated Style:** The `animatedStyle` is created using `useAnimatedStyle` and applies the calculated opacity to the `Animated.View`.

4. **Pointer Events:** The `useAnimatedReaction` hook is used to toggle pointer events on the `Animated.View`. When the opacity is 1 (fully visible), pointer events are set to 'auto', allowing interaction with the content. When the opacity is less than 1 (fading or fully faded), pointer events are set to 'none', preventing interaction. This ensures that the user can't interact with the content when it's not visible.

## Important Considerations

* **`react-native-reanimated` Dependency:**  This component is built using `react-native-reanimated` and its shared values and animations.
* **`esoftplay` Dependency:** This component uses `useSafeState` from `esoftplay`.
* **Scroll Event:** The `scrollController` shared value is typically updated by the `onScroll` event of a `ScrollView`. The `scrollEventThrottle` prop can be used to control how often the `onScroll` event is fired.
* **Interpolation Ranges:** The `interpolateScroll` and `interpolateOpacity` arrays must have the same length and define the mapping between scroll offset and opacity.  The `interpolateScroll` values should be in ascending order.
* **Performance:** Using `react-native-reanimated` and shared values allows for performant animations on the native thread.
* **Pointer Events:**  Disabling pointer events when the content is not fully visible is crucial for a good user experience.  It prevents accidental interactions with the underlying content.
* **Accessibility:** Consider accessibility implications. If the content fades out, ensure that users are aware of its presence and can access it if needed (e.g., using screen readers or alternative interaction methods).
