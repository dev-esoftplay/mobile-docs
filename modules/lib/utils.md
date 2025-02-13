# LibUtils

This module provides a collection of utility functions for common tasks in React Native development. It includes functions for data manipulation, formatting, date/time handling, linking, sharing, and more.

## Installation

This module relies on several external libraries. Ensure you have them installed:

```bash
npm install expo-application expo-clipboard expo-secure-store moment react-fast-compare shorthash buffer
# or
yarn add expo-application expo-clipboard expo-secure-store moment react-fast-compare shorthash buffer
```

You'll also need to install the type definitions for `moment` if you are using TypeScript:

```bash
npm install @types/moment
# or
yarn add @types/moment
```

## Usage

```javascript
import { LibUtils } from 'esoftplay/cache/lib/utils/import';

// Check if a nested object property exists:
const exists = LibUtils.checkUndefined(myObject, 'nested.property.value');

// Debounce a function:
LibUtils.debounce(() => {
  // Function to be debounced
}, 200);

// Format currency:
const formattedAmount = LibUtils.money(1234567.89, 'USD');

// Get URL parameters:
const params = LibUtils.getUrlParams('[https://example.com/?param1=value1&param2=value2](https://www.google.com/search?q=https://example.com/%3Fparam1%3Dvalue1%26param2%3Dvalue2)');

// Get current date and time in a specific format:
const now = LibUtils.getCurrentDateTime('YYYY-MM-DD HH:mm:ss');

// Open a telephone link:
LibUtils.telTo('1234567890');

// ... other utility functions
```

## API

### `LibUtils` (Object with static methods)

*   `checkUndefined(obj: any, cursorsAsString: string): boolean`: Checks if a nested property exists in an object.
*   `debounce(func: () => any, delay: number): void`: Debounces a function.
*   `decodeBase64(chipper: string): string`: Decodes a base64 encoded string.
*   `encodeBase64(plain: string): string`: Encodes a string to base64.
*   `qrGenerate(string: string, size?: number): string`: Generates a QR code URL.
*   `uniqueArray(array: any[]): any[]`: Removes duplicate items from an array.
*   `getArgs(props: any, key: string, defOutput?: any): any`: Gets a navigation parameter.
*   `getArgsAll<S>(props: any, defOutput?: any): S`: Gets all navigation parameters.
*   `objectToUrlParam(obj: any): string`: Converts an object to URL parameters.
*   `moment(date?: string, locale?: any)`: Returns a Moment.js object.
*   `getUrlParams(url: string): any`: Gets URL parameters from a URL string.
*   `getTimezoneByConfig(datetime?: Date | string): string`: Gets datetime by timezone from config.
*   `getRatingValue(rating: string): number`: Gets the calculated rating value.
*   `getRatingCount(rating: string): number`: Gets the rating count.
*   `getRating(rating: string, decimalPlaces?: number): string`: Gets the average rating.
*   `money(value: string | number, currency?: string): string`: Formats a number as currency.
*   `numberAbsolute(toNumber: string | number): number`: Gets the absolute value of a number.
*   `number(toNumber: string | number): string`: Formats a number with commas.
*   `countDays(start: string | Date, end: string | Date): number`: Calculates the number of days between two dates.
*   `getDateTimeSeconds(start: string | Date, end: string | Date): number`: Gets the difference in seconds between two date/times.
*   `ucwords(str: string): string`: Converts a string to title case.
*   `getCurrentDateTime(format?: string): string`: Gets the current date and time in a specified format.
*   `getDateRange(start_date: string, end_date: string, separator?: string, format?: LibUtilsDate): string`: Formats a date range.
*   `getDateAsFormat(input: string, format?: string): string`: Formats a date string.
*   `telTo(number: string | number): void`: Opens a telephone link.
*   `smsTo(number: string | number, message?: string): void`: Opens an SMS link.
*   `mailTo(email: string, subject?: string, message?: string): void`: Opens a mailto link.
*   `waTo(number: string, message?: string): void`: Opens a WhatsApp link.
*   `mapTo(title: string, latlong: string): void`: Opens a map link.
*   `isValidLatLong(latlong: string): boolean`: Checks if a latitude/longitude string is valid.
*   `mapDirectionTo(latlongFrom: string, latlongTo: string, travelmode: LibUtilsTravelMode): void`: Opens a map directions link.
*   `copyToClipboard(string: string): Promise<any>`: Copies a string to the clipboard.
*   `colorAdjust(hex: string, lum: number): string`: Adjusts the luminance of a hex color.
*   `escapeRegExp(str: string): string`: Escapes special characters for regular expressions.
*   `hexToRgba(hex: string, alpha: number): string`: Converts a hex color to RGBA.
*   `shorten(string: string): string`: Shortens a string using a hash.
*   `share(url: string, message?: string): void`: Shares a URL and optional message.
*   `getInstallationID(): Promise<string>`: Gets a unique installation ID.
*   `sprintf(string: string, ...stringToBe: any[]): string`: Formats a string using sprintf-style placeholders.

## Interfaces and Types

### `LibUtilsDate`

```typescript
interface LibUtilsDate {
  year: string,
  month: string,
  date: string,
}
```

### `LibUtilsTravelMode`

```typescript
type LibUtilsTravelMode = 'driving' | 'walking';
```

## Global State

*   `installationId`: Stores a unique installation ID.
*   `cache`: Used internally for the `debounce` function.

## Important Considerations

*   **Dependencies:** Ensure all required libraries are installed.
*   **Moment.js:**  Uses Moment.js for date/time manipulation.  Be sure to set the locale appropriately using `moment().locale(locale)`.
*   **Currency Formatting:** The `money` function formats currency based on the provided locale.
*   **Linking:** The `telTo`, `smsTo`, `mailTo`, `waTo`, and `mapTo` functions open external links.  Handle these actions gracefully in your app.
*   **Clipboard:** The `copyToClipboard` function copies text to the clipboard.
*   **Color Manipulation:** The `colorAdjust` and `hexToRgba` functions provide utilities for working with colors.
*   **Regular Expressions:** The `escapeRegExp` function is useful for escaping strings that will be used in regular expressions.
*   **Sharing:** The `share` function uses the React Native `Share` API.
*   **Installation ID:** The `getInstallationID` function retrieves a unique installation ID.  It uses `expo-secure-store` on iOS and `expo-application` on Android.
*   **String Formatting:** The `sprintf` function provides a way to format strings using placeholders.
* **Global State Management:**  The component uses `useGlobalState` from `esoftplay` to manage the `installationId` and the `cache` used for debouncing.  Global state is useful for data that needs to be accessed and modified from multiple components.  However, be mindful of the potential performance implications of overusing global state.
* **React Fast Compare:** The `react-fast-compare` library is used in the `uniqueArray` function to efficiently compare array items for equality. This improves the performance of the duplicate removal process.
* **Shorthash:** The `shorthash` library is used in the `shorten` function to generate a short hash from a string. This can be useful for creating shortened URLs or other identifiers.
* **Buffer:** The `buffer` library is used for base64 encoding and decoding.
