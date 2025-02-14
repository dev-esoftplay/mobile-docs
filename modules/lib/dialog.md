# LibDialog

This component provides a customizable dialog/modal for React Native. It supports different styles (default, danger), custom views, icons, titles, messages, and action buttons. It uses a global state for managing visibility and properties.

## Installation

This component relies on `esoftplay`. Ensure you have it installed in your project.

```bash
npm install esoftplay
# or
yarn add esoftplay
```

## Usage

```javascript
import { LibDialog } from 'esoftplay/cache/lib/dialog/import';

// Showing a basic info dialog:
LibDialog.info("Information", "This is an information message.");

// Showing a confirmation dialog:
LibDialog.confirm(
  "Confirm",
  "Are you sure you want to do this?",
  "OK",
  () => { /* OK action */ },
  "Cancel",
  () => { /* Cancel action */ }
);

// Showing a custom view dialog:
LibDialog.custom(<MyCustomView />);
```

## API

### Static Methods

The `LibDialog` component provides several static methods for displaying different types of dialogs:

#### **`LibDialog.hide()`**
Hides the currently visible dialog.

#### **`LibDialog.info(title, msg)`**
Shows an information dialog with the given title and message.

#### **`LibDialog.confirm(title, msg, ok, onPressOK, cancel, onPressCancel)`**
Shows a confirmation dialog with the given title, message, OK button text and action, and Cancel button text and action.

#### **`LibDialog.warningConfirm(title, msg, ok, onPressOK, cancel, onPressCancel)`**
Shows a warning confirmation dialog (danger style) with the given parameters.

#### **`LibDialog.failed(title, msg)`**
Shows a failure dialog (danger style) with the given title and message.

#### **`LibDialog.warning(title, msg)`**
Shows a warning dialog (danger style) with the given title and message.

#### **`LibDialog.show(style, icon, title, msg, ok, cancel, onPressOK, onPressCancel)`**
Shows a dialog with the specified style, icon, title, message, OK button text and action, and Cancel button text and action.

#### **`LibDialog.custom(view)`**
Shows a dialog with a custom view.

### Props

The `LibDialog` component itself doesn't directly receive props. The dialog's properties are managed through the static methods and a global state.  The `LibDialog` component renders the `Dialog` component, which uses the global state.

## `Dialog` Component (Internal)

The `Dialog` component is responsible for rendering the actual dialog UI. It accesses the dialog properties from the global state managed by the static methods of `LibDialog`.

## Global State

The dialog's visibility and properties are managed using a global state (from `esoftplay`).  This allows the dialog to be controlled from anywhere in the application using the static methods.

## Styling

The dialog's styling is handled using the `LibTheme`, `LibStyle`, `LibTextstyle`, and `LibIconStyle` modules from `esoftplay`.  The component supports two styles: "default" and "danger".

## Back Handling

The component handles back presses by removing the dialog if it's visible.

## Important Considerations

* **`esoftplay` Dependency:** This component is tightly coupled to the `esoftplay` library.  Make sure it is correctly set up in your project.  All modules should be imported directly from `esoftplay`.
* **Global State:**  The use of a global state makes the dialog easily accessible from anywhere in the app, but it also means that any part of the app can potentially modify the dialog's state.  This might make debugging more challenging in complex applications.  Consider the implications of global state management in your application's architecture.
* **Static Methods:** The static methods provide a convenient way to show different types of dialogs.
* **Custom Views:** The `custom` method allows for great flexibility by letting you render any React element within the dialog.
* **Styling:** The styling relies on `esoftplay`'s styling modules.  Make sure you understand how to use them to customize the dialog's appearance.
* **Back Handling:** The back handling ensures the dialog is dismissed when the user presses the back button.
* **Component Structure:** The separation of `LibDialog` (the class with static methods) and `Dialog` (the functional component rendering the UI) helps to organize the code.
* **Accessibility:** Consider adding accessibility features to the dialog, such as labels and appropriate ARIA attributes.
