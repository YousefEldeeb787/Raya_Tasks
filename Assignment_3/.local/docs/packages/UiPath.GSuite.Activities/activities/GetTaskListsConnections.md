# Get Task Lists

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.GetTaskListsConnections`

Gets all Google Task Lists assigned to the authenticated user. Supports filtering by title.

**Package:** `UiPath.GSuite.Activities`
**Category:** Tasks
**Connector:** `uipath-google-tasks`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Filter` | Filter | `Property` | [`TaskListFilterCollection`](filtering/TaskListFilterCollection.md) | No | | Indicates the filter conditions to be applied to the task lists retrieved. See [TaskListFilterCollection](filtering/TaskListFilterCollection.md) for criteria. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google Workspace connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `TaskLists` | Task Lists | `OutArgument` | `IEnumerable<`[`GTaskList`](types/GTaskList.md)`>` | The retrieved task lists. |

## Output Model

Returns a collection of [`GTaskList`](types/GTaskList.md) objects.

## XAML Example

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:gsuite="clr-namespace:UiPath.GSuite.Activities;assembly=UiPath.GSuite.Activities"
-->
<gsuite:GetTaskListsConnections
    DisplayName="Get Task Lists"
    ConnectionId="{x:Null}"
    TaskLists="[allTaskLists]" />
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- This activity does not use [`ListOrTaskItemArgument`](components/ListOrTaskItemArgument.md). It retrieves all task lists for the authenticated user.
- Use the `Filter` property to narrow results by title. See [`TaskListFilterCollection`](filtering/TaskListFilterCollection.md) for available filter criteria.
