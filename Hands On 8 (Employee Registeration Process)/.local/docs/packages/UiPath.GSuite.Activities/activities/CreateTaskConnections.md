# Create Task

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.CreateTaskConnections`

Creates a new Google Task in the specified task list. Optionally, specify a parent task to create a subtask.

**Package:** `UiPath.GSuite.Activities`
**Category:** Tasks
**Connector:** `uipath-google-tasks`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Task List or Parent Task | `Property` | [`ListOrTaskItemArgument`](components/ListOrTaskItemArgument.md) | Yes | | The task list to create the task in, or a parent task to create a subtask under. See [ListOrTaskItemArgument](components/ListOrTaskItemArgument.md) for input modes. |
| `Title` | Title | `InArgument` | `String` | Yes | | The task title. |
| `Description` | Description | `InArgument` | `String` | No | | The task details/notes. |
| `DueDate` | Due Date | `InArgument` | `DateTime?` | No | | The due date of the task. |
| `Status` | Status | `InArgument` | [`GTaskStatus`](#gtaskstatus) | No | `NeedsAction` | The status of the task. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google Workspace connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Task | `OutArgument` | [`GTask`](types/GTask.md) | The created task. |

## Output Model

Returns a [`GTask`](types/GTask.md) with task ID, title, details, status, due date, and other task metadata.

## Enum Reference

### `GTaskStatus`

| Value | Description |
|-------|-------------|
| `NeedsAction` | The task is not yet completed (default). |
| `Completed` | The task has been marked as completed. |

## XAML Example

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:gsuite="clr-namespace:UiPath.GSuite.Activities;assembly=UiPath.GSuite.Activities"
    xmlns:models="clr-namespace:UiPath.GSuite.Activities.Tasks.Models;assembly=UiPath.GSuite.Activities"
-->
<gsuite:CreateTaskConnections
    DisplayName="Create Task"
    ConnectionId="{x:Null}"
    Title="[taskTitle]"
    Description="[taskDetails]"
    DueDate="[dueDate]"
    Status="[GTaskStatus.NeedsAction]"
    Result="[newTask]">
    <gsuite:CreateTaskConnections.Item>
        <models:ListOrTaskItemArgument InputMode="UrlOrId">
            <models:ListOrTaskItemArgument.ListId>
                <InArgument x:TypeArguments="x:String">[taskListId]</InArgument>
            </models:ListOrTaskItemArgument.ListId>
        </models:ListOrTaskItemArgument>
    </gsuite:CreateTaskConnections.Item>
</gsuite:CreateTaskConnections>
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- The `Title` property is required and will cause a validation error if not set.
- When using `UrlOrId` mode, `ListId` is required. `TaskId` is optional and used to create a subtask under a parent task.
- The default `Status` is `NeedsAction`.
