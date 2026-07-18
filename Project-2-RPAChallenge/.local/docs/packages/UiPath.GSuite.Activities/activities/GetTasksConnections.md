# Get Tasks

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.GetTasksConnections`

Gets a list of tasks/subtasks from a Google Task List or a specific parent task. Supports filtering by title, details, due date, and completed date.

**Package:** `UiPath.GSuite.Activities`
**Category:** Tasks
**Connector:** `uipath-google-tasks`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Task List or Parent Task | `Property` | [`ListOrTaskItemArgument`](components/ListOrTaskItemArgument.md) | Yes | | The task list or parent task to retrieve tasks from. See [ListOrTaskItemArgument](components/ListOrTaskItemArgument.md) for input modes. |
| `IncludeSubtasks` | Include Subtasks | `InArgument` | `Boolean` | No | `false` | Whether to include subtasks in the results. When false, only top-level tasks are returned. |
| `IncludeCompleted` | Include Completed | `InArgument` | `Boolean` | No | `false` | Whether to include completed tasks in the results. |
| `MaxResults` | Max Results | `InArgument` | `Int32` | No | `100` | The maximum number of tasks to return. |
| `Filter` | Filter | `Property` | [`TaskFilterCollection`](filtering/TaskFilterCollection.md) | No | | Indicates the filter conditions to be applied to the tasks retrieved. See [TaskFilterCollection](filtering/TaskFilterCollection.md) for criteria. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google Workspace connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Tasks` | Tasks | `OutArgument` | `IEnumerable<`[`GTask`](types/GTask.md)`>` | The retrieved tasks. |

## Output Model

Returns a collection of [`GTask`](types/GTask.md) objects.

## XAML Example

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:gsuite="clr-namespace:UiPath.GSuite.Activities;assembly=UiPath.GSuite.Activities"
    xmlns:models="clr-namespace:UiPath.GSuite.Activities.Tasks.Models;assembly=UiPath.GSuite.Activities"
-->
<gsuite:GetTasksConnections
    DisplayName="Get Tasks"
    ConnectionId="{x:Null}"
    IncludeCompleted="False"
    MaxResults="[100]"
    Tasks="[taskList]">
    <gsuite:GetTasksConnections.Item>
        <models:ListOrTaskItemArgument InputMode="UrlOrId">
            <models:ListOrTaskItemArgument.ListId>
                <InArgument x:TypeArguments="x:String">[taskListId]</InArgument>
            </models:ListOrTaskItemArgument.ListId>
        </models:ListOrTaskItemArgument>
    </gsuite:GetTasksConnections.Item>
</gsuite:GetTasksConnections>
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- When `IncludeSubtasks` is false (default), only top-level tasks (those without a parent) are returned.
- When a specific task is selected (via `TaskId` or a [`GTask`](types/GTask.md) object), only the subtasks of that task are returned.
- The default `MaxResults` is 100.
- Use the `Filter` property to narrow results. See [`TaskFilterCollection`](filtering/TaskFilterCollection.md) for available filter criteria.
