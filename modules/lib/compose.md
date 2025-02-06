# LibCompose

This module provides a way to dynamically build React Native UIs from a JSON schema. It supports custom components, event handling with `esp` library, form state management, actions, and utilities.

## Installation

This module depends on several libraries, including `esoftplay` library.  You'll need to have these installed in your project.  Specifically, it uses:

* `esoftplay`
* `react-native`
* `react`

Make sure you install them according to their individual instructions.

## Usage

```javascript
import LibCompose from 'esoftplay/cache/lib/compose/import'; // Replace with the correct path

const mySchema = {
  component: 'View',
  props: {
    style: { flex: 1, backgroundColor: 'lightblue' },
    children: [
      {
        component: 'Text',
        props: {
          style: { fontSize: 20 },
          children: 'Hello from JSON!'
        }
      },
      {
        component: 'Button', // Assuming you have a 'Button' component registered
        props: {
          title: 'Press Me',
          onPress: '#esp|lib/utils|alert|["Button Pressed!"]' // Example esp action
        }
      }
    ]
  }
};

<LibCompose schema={mySchema} />;
```

## API

### `LibCompose({ schema })`

The main component that renders the UI from the JSON schema.

* **Parameters:**
    * `schema` (object): The JSON schema object defining the UI.

* **Returns:** A React element representing the built UI.

### `build(cmpn, props)`

A utility function to create a schema object from component and props. This is helpful for programmatically generating schemas.

* **Parameters:**
    * `cmpn` (string): The name of the component (e.g., "View", "Text", or a custom component name).
    * `props` (object): The props for the component.

* **Returns:** A schema object.

### `forms`

An object containing helper functions for creating form-related schema strings.

* `select(postKey, value)`: Creates a schema string for a select input.
* `input(postKey)`: Creates a schema string for a text input.

### `actions`

An object containing helper functions for creating action strings. These actions interact with the `esp` library and other utilities.

* `navigate(module, params)`: Navigates to a different screen.
* `replace(module, params)`: Replaces the current screen.
* `back()`: Navigates back.
* `copy(args)`: Copies text to clipboard.
* `curl(uri, post)`: Makes a network request using `esp`'s curl functionality.

## Schema Structure

The JSON schema is a nested object that defines the UI hierarchy. Each object in the schema should have the following properties:

* `component` (string): The name of the component to render. This can be a standard React Native component (e.g., "View", "Text", "ScrollView") prefixed with `RCT` (e.g. `RCTView`, `RCTText`, `RCTScrollView`), or the name of a custom component registered with `esp`.  If the component name includes a `/` it is interpreted as an `esp` module.
* `props` (object): The props for the component.  Props can include:
    * Standard React Native props.
    * `children`: Either a single child schema object, an array of child schema objects, or a string for text content.
    * Event handlers (e.g., `onPress`, `onChangeText`).  These can be strings that define actions or calls to `esp` functions (see below).

## Dynamic Actions and `esp` Integration

The `LibCompose` component allows you to define dynamic actions within your schema using special string prefixes.

* **`#esp|module|function|arguments`:** Calls a function from an `esp` module.  Arguments should be a JSON array string. Example: `#esp|lib/utils|alert|["Hello!"]`
* **`#espProp|module|function|arguments`:** Calls a function from an `esp` module's properties. Arguments should be a JSON array string. Example: `#espProp|lib/toast|show|["Message displayed!"]`
* **`#action.function.argument`:** Calls a predefined action (see the `action` object). Argument should be a valid JSON string. Example: `#action.navigate.["HomeScreen", { id: 1 }]`
* **`#form.type.key` or `#form.type.key:value`:**  Used for form state management.
    * `#form.input.fieldName`: For text inputs.
    * `#form.select.key:value`: For select inputs.

## Form State Management

`LibCompose` integrates with a global form state using `esoftplay/global`. The `#form.` prefix allows you to connect input components to this global state.

## Example with Form

```javascript
const mySchema = {
  component: 'View',
  props: {
    children: [
      {
        component: 'TextInput',
        props: {
          placeholder: 'Enter your name',
          onChangeText: '#form.input.name'
        }
      },
      {
        component: 'Button',
        props: {
          title: 'Submit',
          onPress: '#action.curl.["/api/submit", "{}"]' // Submits the form data
        }
      }
    ]
  }
};
```

## `build` Function Example

```javascript
const mySchema = build('View', {
  style: { flex: 1 },
  children: [
    build('Text', { children: 'Dynamically built!' })
  ]
});

<LibCompose schema={mySchema} />;
```

## Important Considerations

* **`esoftplay` Dependency:** This module is tightly coupled with the `esoftplay` ecosystem.  You need to have `esoftplay` properly configured in your project.
* **Custom Components:** If you are using custom components, they must be registered with `esp` for `LibCompose` to be able to render them.
* **String Prefixes:** The string prefixes (`#esp|`, `#action.`, `#form.`) are essential for the dynamic functionality of `LibCompose`.  Make sure to use them correctly in your schema.
* **Type Safety:** Consider using TypeScript or Flow to ensure type safety in your schemas and component definitions.
* **Error Handling:**  The provided code snippet doesn't include robust error handling. You might need to add error handling for invalid schemas or failed `esp` calls.
* **Performance:** For very complex UIs, consider optimizing your schema and components to avoid performance issues.