# LibLocale

This module provides utilities for managing localization in a React Native application. It uses global state (from `esoftplay`) to store the current language and language data, and provides functions for setting the language, retrieving language data, and getting the device's language.

## Installation

This module relies on `esoftplay`. Ensure you have it installed:

```bash
npm install esoftplay
# or
yarn add esoftplay
```

## Usage

```javascript
import { LibLocale } from 'esoftplay/cache/lib/locale/import';

// Get the current language:
const currentLanguage = LibLocale.state().get();

// Get language data for a specific language:
const languageData = LibLocale.stateLang().get();
const englishData = languageData.en; // Accessing English language data

// Set the language:
LibLocale.setLanguage('en');

// Set language data:
LibLocale.setLanguageData('en', { welcome: 'Welcome!' });

// Get the device's language:
const deviceLanguage = LibLocale.getDeviceLanguange();
```

## API

### `state()`

```typescript
state(): useGlobalReturn<any>
```

Returns the global state object for the current language ID.  Use `.get()` to retrieve the current language ID, and `.set()` to change it.

### `stateLang()`

```typescript
stateLang(): useGlobalReturn<any>
```

Returns the global state object for the language data.  This object stores the translation data for each language. Use `.get()` to retrieve the language data, and `.set()` to modify it.

### `setLanguageData(langId: string, data: any)`

```typescript
setLanguageData(langId: string, data: any): void
```

Sets the language data for a specific language ID.  The `data` argument should be an object containing the translation strings for that language.  This function updates the global state for language data.

### `setLanguage(langId: string)`

```typescript
setLanguage(langId: string): void
```

Sets the current language.  This function updates the global state for the current language ID and calls `LibNavigation.reset()` which you'll probably want to have configured to reload your app's main view or navigation stack so the changes take effect.

### `getDeviceLanguange()`

```typescript
getDeviceLanguange(): string
```

Returns the device's language code (e.g., "en", "id").  It handles differences between iOS and Android in how the locale is retrieved.  It defaults to "id" (Indonesian) if the detected language is "in".

## Functionality

1. **Global State:** The module uses `useGlobalState` from `esoftplay` to manage the current language ID and the language data.  These values are persisted using `persistKey` so they are preserved across app restarts.  The language data is also stored `inFile`.

2. **Language Data Storage:** The `lang` state object stores the translation strings for each language.  It's an object where the keys are language IDs (e.g., "en", "id") and the values are objects containing the key-value pairs of translation strings.

3. **Setting Language Data:** The `setLanguageData` function allows you to add or update translation data for a specific language.

4. **Setting Language:** The `setLanguage` function sets the current language ID.  It also calls `LibNavigation.reset()`.

5. **Device Language:** The `getDeviceLanguange` function retrieves the device's language code, normalizing it to a two-letter code and providing a default if the language cannot be determined.

## Important Considerations

* **`esoftplay` Dependency:** This module is tightly coupled to `esoftplay` for global state management and navigation.
* **Persistence:** The language and language data are persisted using `persistKey`.
* **Language Data Structure:** The language data is stored as an object of objects.  The outer keys are the language IDs, and the inner objects contain the translation strings.
* **`LibNavigation.reset()`:** The `setLanguage` function calls `LibNavigation.reset()`.  You'll almost certainly need to configure this to reload your app's content or navigation so that the language change takes effect.
* **Device Language:** The `getDeviceLanguange` function provides a best-effort attempt to determine the device's language.  However, it might not be completely accurate in all cases.
* **Error Handling:**  The module doesn't include explicit error handling.  Consider adding error handling for cases where the language data is not found or the device language cannot be determined.
* **Internationalization (i18n) Libraries:** For more complex internationalization needs (e.g., pluralization, date/time formatting, message formatting), consider using a dedicated i18n library like `react-i18next` or `FormatJS`.  This module provides a basic foundation for managing language data and the current language, but a dedicated i18n library is recommended for more advanced scenarios.
