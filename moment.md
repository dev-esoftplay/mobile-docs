# Moment Utility Functions Documentation

This document describes a set of utility functions for working with dates and times, built upon `dayjs` and integrated with the `esoftplay` framework.

## Installation

These functions rely on `dayjs` and its plugins, as well as `esoftplay` utilities. Ensure you have the necessary dependencies installed:

```bash
npm install dayjs
# And any dayjs plugins you need, e.g.:
npm install dayjs/plugin/utc
npm install dayjs/plugin/timezone
npm install dayjs/plugin/relativeTime
npm install dayjs/plugin/duration
# And of course, esoftplay
```

## Usage

```javascript
import moment, { setTimeOffset, resetTimeOffset, useLocalFormatOnline } from './path/to/your/momentUtils'; // Adjust path

// Formatting a date
const formattedDate = moment().format('YYYY-MM-DD HH:mm:ss');

// Adding time
const futureDate = moment().add(7, 'days').format('YYYY-MM-DD');

// Getting time difference
const duration = moment('2023-10-26').duration('2023-10-28');
const daysDifference = duration.days();

// Set and reset Time Offset
const dateWithOffset = setTimeOffset(new Date())
const originalDate = resetTimeOffset(dateWithOffset)

// Use online time for format with fallback to local
const [onlineFormattedDate, getOnlineTime] = useLocalFormatOnline('YYYY-MM-DD HH:mm:ss');

// ... other functionalities
```

## API Reference

### `setTimeOffset(date?: string | Date | any)`

Sets the time offset to Asia/Jakarta (GMT+7).

* **Parameters:**
    * `date?`: The date to adjust. Defaults to the current date and time. Can be a `Date` object, a string parsable by `new Date()`, or a timestamp (numeric).
* **Returns:** A new `Date` object with the Asia/Jakarta offset.

### `resetTimeOffset(date: Date)`

Resets the time offset from Asia/Jakarta (GMT+7) to the original time.

* **Parameters:**
    * `date`: The date to reset. Can be a `Date` object or a string parsable by `dayjs`.
* **Returns:** A new `Date` object with the original time.

### `useLocalFormatOnline(custom)`

A React hook that formats the current time using an online time source (worldtimeapi.org) as the primary source and the device's local time as a fallback.

* **Parameters:**
    * `custom`: The format string to use with `dayjs`.
* **Returns:** An array containing:
    1. `date`: The formatted date string. This value is updated whenever the online time is fetched successfully or the fallback to local time occurs.
    2. `get`: A function that triggers a refetch of the online time. Useful for refreshing the displayed time.

### `moment(date?: string | Date | any)`

The main function for working with dates and times, inspired by Moment.js.

* **Parameters:**
    * `date?`: The date to work with.  Can be a `Date` object, a string parsable by `dayjs`, a timestamp (numeric), or nothing (to represent the current date and time).
* **Returns:** An object with methods for manipulating and formatting the date.

#### `moment` object methods:

* **`add(count: number, type: "days" | "weeks" | "months" | "quarters" | "years" | "hours" | "minutes" | "seconds" | "milliseconds")`:** Adds a specified amount of time to the date.
* **`subtract(count: number, type: "days" | "weeks" | "months" | "quarters" | "years" | "hours" | "minutes" | "seconds" | "milliseconds")`:** Subtracts a specified amount of time from the date.
* **`tz(timeZone: string)`:** Sets the time zone.
* **`locale(locale_id: string)`:** Sets the locale.  Loads the necessary locale files (`en`, `id`, `zh-cn`).
* **`fromNow()`:** Returns a human-readable relative time (e.g., "a few seconds ago"). Uses the current locale.
* **`format(custom: string, ignoreTimezone?: boolean)`:** Formats the date according to the specified custom format string.  Applies the current locale.  If `ignoreTimezone` is true, the date is formatted without timezone adjustment.
* **`serverFormat(custom: string)`:** Formats the date for server use (typically with timezone adjustment).
* **`localeFormat(custom: string, ignoreTimezone?: boolean)`:** Formats the date using locale specific format.
* **`toDate()`:** Converts the `dayjs` object to a native `Date` object.
* **`toMiliseconds()`:** Returns the timestamp in milliseconds as a string.
* **`duration(other_date: string | Date)`:** Calculates the duration between two dates.

## Type Definitions

* `momentType`: An array of strings representing the time units (days, weeks, etc.).
* `dayjsType`: An array of strings representing the `dayjs` equivalent time units.

## Helper Functions

* `isNumeric(n)`: Checks if a value is numeric.

## Example (Combined)

```javascript
import moment, { setTimeOffset, resetTimeOffset } from './path/to/your/momentUtils';

const now = moment();
const future = now.add(1, 'week');
const formatted = future.format('YYYY-MM-DD HH:mm');
const relative = now.fromNow();

console.log(formatted); // e.g., 2023-11-03 10:00
console.log(relative); // e.g., a few seconds ago

const jakartaTime = setTimeOffset(new Date());
const originalTime = resetTimeOffset(jakartaTime);

console.log("Jakarta Time:", jakartaTime);
console.log("Original Time:", originalTime);

const duration = moment('2023-10-26').duration('2023-10-28');
const daysDifference = duration.days();
console.log("Days Difference:", daysDifference);
```

This documentation should help you understand and use the provided date/time utility functions effectively. Remember to adjust the import paths according to your project structure.
