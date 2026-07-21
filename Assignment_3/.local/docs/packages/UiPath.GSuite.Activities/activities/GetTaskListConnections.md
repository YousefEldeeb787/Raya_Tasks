# Get Task List

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.GetTaskListConnections`

Retrieves a single Google Task List by its identifier.

**Package:** `UiPath.GSuite.Activities`
**Category:** Tasks
**Connector:** `uipath-google-tasks`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Identifier` | Identifier | `InArgument` | `String` | Yes | | The task list identifier (ID or base64-encoded task list object). |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google Workspace connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `TaskList` | Task List | `OutArgument` | [`GTaskList`](types/GTaskList.md) | The retrieved task list. |

## Output Model

Returns a [`GTaskList`](types/GTaskList.md) with list ID, title, URL, and last modified date.

## XAML Example

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:gsuite="clr-namespace:UiPath.GSuite.Activities;assembly=UiPath.GSuite.Activities"
-->
<gsuite:GetTaskListConnections
    DisplayName="Get Task List"
    ConnectionId="{x:Null}"
    Identifier="[taskListId]"
    TaskList="[retrievedTaskList]" />
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- This activity does not use [`ListOrTaskItemArgument`](components/ListOrTaskItemArgument.md). It takes a simple string identifier.
- The `Identifier` can be a task list ID string or a base64-encoded `GTaskListSlim` object.
