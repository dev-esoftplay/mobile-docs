# esp

This document describes the `esp` library, a utility library for React Native applications.  It provides a set of helper functions for accessing assets, configuration, modules, language resources, and more.

## Installation

This library is part of the `esoftplay` ecosystem and is likely included as a dependency in projects using it.  Consult the `esoftplay` documentation for specific installation instructions.

## Usage

```javascript
import esp from 'esoftplay/esp';

// Accessing assets
const background = esp.assets("background");

// Accessing configuration values
const dataName = esp.config("data", "name");

// Executing a module/task
const moduleTaskResult = esp.mod("module/task");

// Executing the home module/task
const homeResult = esp.home();

// Logging a message (only in debug mode)
esp.log("A message to log");

// Accessing navigation history
const navigationState = esp.routes();

// Accessing language resources
const welcomeMessage = esp.lang("module/task", "welcome");

// ... other functionalities
```

## API Reference

### `esp.mergeDeep(target, ...sources)`

Recursively merges multiple source objects into a target object.

* **Parameters:**
    * `target`: The target object.
    * `...sources`: The source objects to merge.
* **Returns:** The merged object.

### `esp.appjson()`

Merges `app.json` and `config.json` into a single object.

* **Returns:** The merged JSON object.

### `esp.assets(path)`

Retrieves assets from the specified path.  Uses the `./assets` folder.

* **Parameters:**
    * `path`: The path to the asset.
* **Returns:** The asset data.

### `esp.versionName()`

Returns the application version name (build number) combined with the publish ID.

* **Returns:** The version name string.

### `esp.config(param?, ...params)`

Retrieves configuration values from `config.json`.

* **Parameters:**
    * `param?`: The first key in the configuration path.
    * `...params`: Additional keys in the configuration path.
* **Returns:** The configuration value or an empty object if the path is invalid.

### `esp.isDebug(message)`

Checks if the application is in debug mode based on the domain configuration.

* **Parameters:**
    * `message`: A message (not currently used).
* **Returns:** `true` if in debug mode, `false` otherwise.

### `esp.readDeepObj(obj)`

Creates a function to read nested properties of an object.

* **Parameters:**
    * `obj`: The object to read from.
* **Returns:** A function that takes a path (as a series of strings) and returns the value at that path.

### `esp.lang(moduleTask, langName, ...stringToBe)`

Retrieves a localized string.  Uses `esoftplay/cache/lib/locale/import`.

* **Parameters:**
    * `moduleTask`: The module/task key for the language resource.
    * `langName`: The name of the language string.
    * `...stringToBe`: Values to replace placeholders (`%s`) in the string.
* **Returns:** The localized string.

### `esp.langId()`

Gets the current language ID.

* **Returns:** The language ID string.

### `esp.mod(path)`

Loads a module/task. Uses `esoftplay/cache/routers`.

* **Parameters:**
    * `path`: The path to the module/task.
* **Returns:** The loaded module/task.

### `esp.modProp(path)`

Loads a module/task property. Uses `esoftplay/cache/properties`.

* **Parameters:**
    * `path`: The path to the module/task property.
* **Returns:** The loaded module/task property.

### `esp._config()`

Internal function to load and process the application configuration.

* **Returns:** The processed configuration object.

### `esp.navigations()`

Retrieves the navigation configuration. Uses `esoftplay/cache/navs`.

* **Returns:** An array of navigation items.

### `esp.home()`

Executes the home module/task.

* **Returns:** The result of the home module/task.

### `esp.routes()`

Gets the current navigation state.

* **Returns:** The navigation state object.

### `esp.log(message?, ...optionalParams)`

Logs a message to the console (only in debug mode).

* **Parameters:**
    * `message?`: The message to log.
    * `...optionalParams`: Additional parameters to log.
* **Returns:** `void`

### `esp.condition()`

Creates a conditional execution API.

* **Returns:** An object with `if`, `elseif`, `else`, and `getValue` methods.

### `esp.logColor`

An object containing ANSI escape codes for colored console output.

## Type Definitions

* `ConfigType`:  Type of `cacheConfig`.

## Constants

* `ignoreWarns`: An array of warning messages to ignore.

## Example Usage (Combined)

```javascript
import esp from './path/to/esp';

const appVersion = esp.versionName();
const homeModule = esp.home();
const welcome = esp.lang("module/task", "welcome", "User");
const isDev = esp.isDebug();

if (isDev) {
  esp.logColor.red("Debug Mode Enabled!");
  esp.log("App Version:", appVersion);
}

console.log("Welcome Message:", welcome);
console.log("Home Module:", homeModule);

const config = esp.config();
console.log("Full Config:", config);

const dataName = esp.config("data", "name");
console.log("Data Name:", dataName);


```
