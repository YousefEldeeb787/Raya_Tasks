# Rename Task List

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.RenameTaskListConnections`

Renames an existing Google Task List by updating its title.

**Package:** `UiPath.GSuite.Activities`
**Category:** Tasks
**Connector:** `uipath-google-tasks`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Task List | `Property` | [`ListOrTaskItemArgument`](components/ListOrTaskItemArgument.md) | Yes | | The task list to rename. See [ListOrTaskItemArgument](components/ListOrTaskItemArgument.md) for input modes. |
| `Title` | Title | `InArgument` | `String` | Yes | | The new title for the task list. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google Workspace connection to use. |

## XAML Example

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:gsuite="clr-namespace:UiPath.GSuite.Activities;assembly=UiPath.GSuite.Activities"
    xmlns:models="clr-namespace:UiPath.GSuite.Activities.Tasks.Models;assembly=UiPath.GSuite.Activities"
-->
<gsuite:RenameTaskListConnections
    DisplayName="Rename Task List"
    ConnectionId="{x:Null}"
    Title="[newTitle]">
    <gsuite:RenameTaskListConnections.Item>
        <models:ListOrTaskItemArgument InputMode="UseExisting">
            <models:ListOrTaskItemArgument.ListOrTask>
                <InArgument x:TypeArguments="tasks:ITaskItem">[existingTaskList]</InArgument>
            </models:ListOrTaskItemArgument.ListOrTask>
        </models:ListOrTaskItemArgument>
    </gsuite:RenameTaskListConnections.Item>
</gsuite:RenameTaskListConnections>
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- This activity targets a task list (not an individual task). A [`ListOrTaskItemArgument`](components/ListOrTaskItemArgument.md) is used, requiring only `ListId` in `UrlOrId` mode.
- The default `TaskInputMode` is `UseExisting`.
- If a [`GTask`](types/GTask.md) object is passed instead of a [`GTaskList`](types/GTaskList.md), the activity throws an error because it expects a task list, not a task.
- This activity has no output properties.
