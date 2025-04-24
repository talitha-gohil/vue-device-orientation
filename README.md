# vue-device-orientation

Vue 3 composable to detect where the user is on a mobile device, and whether that device is in portrait mode. This can be useful for instructing users to rotate their phone to landscape. Device detection is based on `viewportWidth`, `devicePixelRatio`, and `screen.orientation`.
## Features
- **Custom Config**: You can pass a configuration object to customise the maximum width that determines whether the device is considered mobile.
- **Orientation Change Handling**: Supports detecting orientation changes and provides the option of passing a callback to be executed after the orientation change has completed.
- **Resize Support**: Listens to window resizing and updates the device state accordingly.

## Installation

You can install the package via npm:

```bash
npm install vue-device-orientation
```

## Basic usage
Call the `onOrientationChange` method manually from an **orientationchange** event listener. This event listener is defined in the component instead of the composable so that we have the option of passing an additional method to it when needed (see 'Orientation Change Callback').

```html
<script setup>
import { useDeviceOrientation } from "vue-device-orientation";
const { isMobile, isMobileAndPortrait, orientationLoading, onOrientationChange } = useDeviceOrientation();

onMounted(() => {
    window.addEventListener("orientationchange", () => onOrientationChange());
});

onUnmounted(() => {
    window.removeEventListener("orientationchange", () => onOrientationChange());
});
</script>

<template>
  <div>
    <p>Is Mobile: {{ isMobile }}</p>
    <p>Is Mobile and Portrait: {{ isMobileAndPortrait }}</p>
    <p v-if="orientationLoading">Updating orientation...</p>
  </div>
</template>
```

### Custom config
You can pass a custom configuration object to override the default `deviceMaxWidth` (default is 600px). The width is calculated by dividing the `viewportWidth` by the `devicePixelRatio`.

```html
<script setup>
import { useDeviceOrientation } from "vue-device-orientation";

// Initialize with a custom max width for mobile devices
const { isMobile, isMobileAndPortrait, orientationLoading, onOrientationChange } = useDeviceOrientation({
  deviceMaxWidth: 500,
});
</script>
```

### Orientation change callback
You can optionally provide additional functionality via a callback to be executed when the orientation changes, such as a method which relies on the new screen width.

```html
<script setup>
import { useDeviceOrientation } from "vue-device-orientation";

const { onOrientationChange } = useDeviceOrientation();

onMounted(() => {
    window.addEventListener("orientationchange", () => onOrientationChange(myExtraFunction));
});

onUnmounted(() => {
    window.removeEventListener("orientationchange", () => onOrientationChange(myExtraFunction));
});
</script>
```

## API
`useDeviceOrientation(config: DeviceOrientationConfig = {})`

### Parmeters
• `deviceMaxWidth (number)`: The maximum width (in pixels) for determining if the device is considered mobile. The default value is 600.

### Returns
• `isMobile`: Device is mobile (screen width is less than or equal to the `deviceMaxWidth`, taking into account `devicePixelRatio`).

• `isMobileAndPortrait`: Device is mobile and in portrait mode.

• `orientationLoading`: Device orientation is being updated.

• `onOrientationChange(cb?: () => void)`: Recalculates `isMobile` and `isMobilePortrait` values. An optional callback can be provided to be executed after these recalculations have completed.


## License
MIT
