# Update Task

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.UpdateTaskConnections`

Updates an existing Google Task's properties such as title, description, due date, or status.

**Package:** `UiPath.GSuite.Activities`
**Category:** Tasks
**Connector:** `uipath-google-tasks`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Task | `Property` | [`TaskItemArgument`](components/TaskItemArgument.md) | Yes | | The task to update. See [TaskItemArgument](components/TaskItemArgument.md) for input modes. |
| `Title` | Title | `InArgument` | `String` | No | | The updated title of the task. |
| `Description` | Description | `InArgument` | `String` | No | | The updated description/notes for the task. |
| `DueDate` | Due Date | `InArgument` | `DateTime?` | No | | The updated due date of the task. |
| `Status` | Status | `InArgument` | [`GTaskStatus`](#gtaskstatus) | No | | The updated status of the task. Only applied if explicitly set. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google Workspace connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Task | `OutArgument` | [`GTask`](types/GTask.md) | The updated task. |

## Output Model

Returns a [`GTask`](types/GTask.md) with updated task properties.

## Enum Reference

### `GTaskStatus`

| Value | Description |
|-------|-------------|
| `NeedsAction` | The task is not yet completed. |
| `Completed` | The task has been marked as completed. |

## XAML Example

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:gsuite="clr-namespace:UiPath.GSuite.Activities;assembly=UiPath.GSuite.Activities"
    xmlns:models="clr-namespace:UiPath.GSuite.Activities.Tasks.Models;assembly=UiPath.GSuite.Activities"
-->
<gsuite:UpdateTaskConnections
    DisplayName="Update Task"
    ConnectionId="{x:Null}"
    Title="[updatedTitle]"
    Status="[GTaskStatus.Completed]"
    Result="[updatedTask]">
    <gsuite:UpdateTaskConnections.Item>
        <models:TaskItemArgument InputMode="UseExisting">
            <models:TaskItemArgument.ListOrTask>
                <InArgument x:TypeArguments="tasks:ITaskItem">[existingTask]</InArgument>
            </models:TaskItemArgument.ListOrTask>
        </models:TaskItemArgument>
    </gsuite:UpdateTaskConnections.Item>
</gsuite:UpdateTaskConnections>
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- This activity targets a specific task (not a task list). A [`TaskItemArgument`](components/TaskItemArgument.md) is used, requiring both `ListId` and `TaskId` in `UrlOrId` mode.
- The default `TaskInputMode` is `UseExisting`.
- If a [`GTaskList`](types/GTaskList.md) object is passed instead of a [`GTask`](types/GTask.md), the activity throws an error because it cannot update a task list.
- The `Status` property is only applied if its expression is explicitly set.
