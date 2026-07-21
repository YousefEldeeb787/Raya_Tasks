# Create Task List

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.CreateTaskListConnections`

Creates a new Google Task List, optionally populating it with tasks from a DataTable or a list of [`GTask`](types/GTask.md) objects.

**Package:** `UiPath.GSuite.Activities`
**Category:** Tasks
**Connector:** `uipath-google-tasks`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Title` | Title | `InArgument` | `String` | Yes | | The title of the new task list. |
| `DataType` | Data Type | `Property` | [`TasksInputType`](#tasksinputtype) | No | `DataTable` | Indicates the type of data used to insert tasks. |
| `TasksDataTable` | Tasks DataTable | `InArgument` | `DataTable` | No | | A DataTable containing tasks to insert. Used when `DataType` is `DataTable`. Columns: Title, Description, Due, Status. |
| `TasksList` | Tasks List | `InArgument` | `IEnumerable<`[`GTask`](types/GTask.md)`>` | No | | A list of [`GTask`](types/GTask.md) objects to insert. Used when `DataType` is `List`. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google Workspace connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `TaskList` | Task List | `OutArgument` | [`GTaskList`](types/GTaskList.md) | The newly created task list. |
| `Tasks` | Tasks | `OutArgument` | `IEnumerable<`[`GTask`](types/GTask.md)`>` | The list of tasks that were inserted into the new task list. |

## Output Model

Returns a [`GTaskList`](types/GTaskList.md) and optionally a collection of [`GTask`](types/GTask.md) objects for the inserted tasks.

## Enum Reference

### `TasksInputType`

| Value | Description |
|-------|-------------|
| `DataTable` | Use a DataTable with columns: Title, Description, Due, Status. |
| `List` | Use a list of [`GTask`](types/GTask.md) objects. |

## XAML Example

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:gsuite="clr-namespace:UiPath.GSuite.Activities;assembly=UiPath.GSuite.Activities"
-->
<!-- Create a task list with no initial tasks -->
<gsuite:CreateTaskListConnections
    DisplayName="Create Task List"
    ConnectionId="{x:Null}"
    Title="[listTitle]"
    TaskList="[newTaskList]"
    Tasks="[createdTasks]" />
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- This activity does not use [`ListOrTaskItemArgument`](components/ListOrTaskItemArgument.md). It creates a new task list from scratch.
- The `Title` property is required and will cause a validation error if not set.
- When using `DataType = DataTable`, the DataTable must have columns named `Title`, `Description`, `Due`, and `Status`.
- When using `DataType = List`, provide a collection of [`GTask`](types/GTask.md) objects via `TasksList`.
