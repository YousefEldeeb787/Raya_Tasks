# ListOrTaskItemArgument

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Activities.Tasks.Models.ListOrTaskItemArgument`

A composition component used by Google Tasks activities to specify a target task list or task. Supports multiple input modes for identifying items by browsing, URL/ID, existing variable, or full path.

**Assembly:** `UiPath.GSuite.Activities`

## InputMode (`EDriveItemMode`)

Reuses the Drive item mode enum. Determines which properties are active.

| Mode | Value | Description | AI-XAML Suitable |
|------|-------|-------------|------------------|
| `Browse` | `0` | Select a task list or task from the remote browser in Studio. | **Not suitable for AI-generated XAML** -- requires interactive Studio UI. |
| `UrlOrId` | `1` | Manually enter list ID and optionally task ID. | Yes |
| `UseExisting` | `2` | Use an existing [GTaskList](../types/GTaskList.md) or [GTask](../types/GTask.md) variable. | Yes |
| `FullPath` | `3` | Enter the absolute path (e.g., "My list/first task"). | Yes |

## Properties

| Property | Type | Mode(s) | Description |
|----------|------|---------|-------------|
| `InputMode` | `EDriveItemMode` | All | How the task list or task is specified. |
| `ListId` | `InArgument<string>` | UrlOrId | List ID manually entered. |
| `TaskId` | `InArgument<string>` | UrlOrId | Task ID manually entered. |
| `ListBrowserId` | `InArgument<string>` | Browse | List ID saved when browsing. |
| `ListFriendlyName` | `InArgument<string>` | Browse | Friendly name of the browsed list. |
| `TaskBrowserId` | `InArgument<string>` | Browse | Task ID saved when browsing. |
| `TaskFriendlyName` | `InArgument<string>` | Browse | Friendly name of the browsed task. |
| `BrowserFullPath` | `InArgument<string>` | Browse | The absolute path as specified by the browser (used for fallback resolution). |
| `FullPath` | `InArgument<string>` | FullPath | The absolute path of the list or task as provided by the user. |
| `ListOrTask` | `InArgument<ITaskItem>` | UseExisting | An existing [GTaskList](../types/GTaskList.md) or [GTask](../types/GTask.md) variable. |
| `ConnectionKey` | `string` | All | The ID of the connection from the moment the connection data was chosen. |
| `ConnectionDescriptor` | `string` | All | A user-friendly string describing the connection. |
| `Backup` | `BackupSlot<EDriveItemMode>` | All | Stores the previous InputMode value for designer revert. |

## XAML Examples

### UrlOrId Mode

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:gsuite="clr-namespace:UiPath.GSuite.Activities;assembly=UiPath.GSuite.Activities"
-->
<ListOrTaskItemArgument InputMode="UrlOrId">
  <ListOrTaskItemArgument.ListId>
    <InArgument x:TypeArguments="x:String">list-id-123</InArgument>
  </ListOrTaskItemArgument.ListId>
</ListOrTaskItemArgument>
```

### UseExisting Mode

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:gsuite="clr-namespace:UiPath.GSuite.Activities;assembly=UiPath.GSuite.Activities"
-->
<ListOrTaskItemArgument InputMode="UseExisting">
  <ListOrTaskItemArgument.ListOrTask>
    <InArgument x:TypeArguments="tasks:ITaskItem">[MyTaskListVariable]</InArgument>
  </ListOrTaskItemArgument.ListOrTask>
</ListOrTaskItemArgument>
```

### FullPath Mode

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:gsuite="clr-namespace:UiPath.GSuite.Activities;assembly=UiPath.GSuite.Activities"
-->
<ListOrTaskItemArgument InputMode="FullPath">
  <ListOrTaskItemArgument.FullPath>
    <InArgument x:TypeArguments="x:String">My Tasks/Buy groceries</InArgument>
  </ListOrTaskItemArgument.FullPath>
</ListOrTaskItemArgument>
```

## Notes

- When `UseExisting` mode receives a [GTask](../types/GTask.md), it extracts both the `ParentListId` and `Id` for resolution. When it receives a [GTaskList](../types/GTaskList.md), only the list `Id` is used.
- `ListId`, `TaskId`, `ListBrowserId`, and `TaskBrowserId` have the `[AutopilotIgnored]` attribute -- they are regular properties but are typically populated by the Studio browser or explicit ID entry, not inferred by AI.
- When the connection changes, Browse mode attempts to re-resolve by verifying the list ID, then falls back to path resolution.

## Used By

Google Tasks activities that need a task list or task reference -- see activity docs for details.
