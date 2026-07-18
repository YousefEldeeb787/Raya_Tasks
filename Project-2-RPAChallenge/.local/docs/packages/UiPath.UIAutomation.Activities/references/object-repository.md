# Object Repository

The Object Repository is a centralized repository for UI Automation screens and targets. It allows you to define and manage reusable UI descriptors (applications, screens, and elements) that can be referenced across your automation project.

**IMPORTANT: All interaction with the Object Repository must be performed via the CLI. The CLI entry point is `uip rpa uia object-repository` (e.g., `uip rpa uia object-repository get-screens`). See [cli-reference.md](cli-reference.md) for the complete CLI reference.**

## Key Concepts

### Application

An **Application** is a top-level container of screens. It represents the application being automated (e.g., a desktop app or a web browser page).

### Screen

A **Screen** represents an application screen. It contains a **TargetApp** which is used by scope activities such as `Use Application/Browser`. Screens are created from definition files that contain window selectors.

### Element

An **Element** represents a UI element on a screen. It contains a **TargetAnchorable** which is used by activities that need a target (e.g., `Click`, `Type Into`). Elements are created from definition files and are always associated with a parent screen.

## CLI Commands

For the full CLI reference including all options, flags, and examples, see [cli-reference.md](cli-reference.md).

The Object Repository CLI commands are organized into four groups:

### Applications

| Command | Description |
|---------|-------------|
| `create-app` | Creates an application entry in the Object Repository. |
| `get-apps` | Gets all applications from the Object Repository. |
| `get-app` | Gets an application by reference ID. |
| `delete-app` | Deletes an application entry. |

### Screens

| Command | Description |
|---------|-------------|
| `create-screen` | Creates a screen entry from a definition file. |
| `get-screens` | Gets screens, optionally filtered by definition file or application. |
| `get-screen` | Gets a screen by reference ID. |
| `get-screen-xaml` | Gets the XAML representation of a screen. |
| `delete-screen` | Deletes a screen entry. |

### Elements

| Command | Description |
|---------|-------------|
| `create-elements` | Creates multiple element entries from definition files in a single batch. |
| `get-elements` | Gets elements for a given screen. |
| `get-element` | Gets an element by reference ID. |
| `get-elements-xaml` | Gets the XAML representation of multiple elements in a single batch. |
| `delete-element` | Deletes an element entry. |

### Linking

| Command | Description |
|---------|-------------|
| `link-elements` | Links Object Repository elements to activities in the XAML workflow. |
| `link-screen` | Links an Object Repository screen to an activity in the XAML workflow. |
