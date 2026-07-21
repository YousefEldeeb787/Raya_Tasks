# Get Task

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.GetTaskConnections`

Retrieves a single Google Task by its identifier, optionally including its subtasks.

**Package:** `UiPath.GSuite.Activities`
**Category:** Tasks
**Connector:** `uipath-google-tasks`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Task | `Property` | [`TaskItemArgument`](components/TaskItemArgument.md) | Yes | | The task to retrieve. See [TaskItemArgument](components/TaskItemArgument.md) for input modes. |
| `IncludeSubtasks` | Include Subtasks | `InArgument` | `Boolean` | No | `false` | Specify whether to include subtasks or not. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google Workspace connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `ResultTask` | Task | `OutArgument` | [`GTask`](types/GTask.md) | The retrieved task. |

## Output Model

Returns a [`GTask`](types/GTask.md) with task ID, title, details, status, due date, and other task metadata.

## XAML Example

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:gsuite="clr-namespace:UiPath.GSuite.Activities;assembly=UiPath.GSuite.Activities"
    xmlns:models="clr-namespace:UiPath.GSuite.Activities.Tasks.Models;assembly=UiPath.GSuite.Activities"
-->
<gsuite:GetTaskConnections
    DisplayName="Get Task"
    ConnectionId="{x:Null}"
    IncludeSubtasks="True"
    ResultTask="[retrievedTask]">
    <gsuite:GetTaskConnections.Item>
        <models:TaskItemArgument InputMode="UrlOrId">
            <models:TaskItemArgument.ListId>
                <InArgument x:TypeArguments="x:String">[taskListId]</InArgument>
            </models:TaskItemArgument.ListId>
            <models:TaskItemArgument.TaskId>
                <InArgument x:TypeArguments="x:String">[taskId]</InArgument>
            </models:TaskItemArgument.TaskId>
        </models:TaskItemArgument>
    </gsuite:GetTaskConnections.Item>
</gsuite:GetTaskConnections>
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- This activity targets a specific task (not a task list). A [`TaskItemArgument`](components/TaskItemArgument.md) is used, requiring both `ListId` and `TaskId` in `UrlOrId` mode.
- If a [`GTaskList`](types/GTaskList.md) object is passed instead of a [`GTask`](types/GTask.md), the activity throws an error because it cannot retrieve a task list as a task.
