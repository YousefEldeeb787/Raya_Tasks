# TaskItemArgument

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Activities.Tasks.Models.TaskItemArgument`

A composition component used by Google Tasks activities that require a specific task (not just a task list). Extends [ListOrTaskItemArgument](ListOrTaskItemArgument.md) with stricter validation that ensures a task is identified, not just a list.

**Assembly:** `UiPath.GSuite.Activities`
**Inherits:** [`ListOrTaskItemArgument`](ListOrTaskItemArgument.md)

## InputMode (`EDriveItemMode`)

Same as [ListOrTaskItemArgument](ListOrTaskItemArgument.md).

| Mode | Value | Description | AI-XAML Suitable |
|------|-------|-------------|------------------|
| `Browse` | `0` | Select a task from the remote browser in Studio. | **Not suitable for AI-generated XAML** -- requires interactive Studio UI. |
| `UrlOrId` | `1` | Manually enter both list ID and task ID. | Yes |
| `UseExisting` | `2` | Use an existing [GTask](../types/GTask.md) variable. | Yes |
| `FullPath` | `3` | Enter the absolute path (e.g., "My list/first task"). | Yes |

## Properties

All properties are inherited from [ListOrTaskItemArgument](ListOrTaskItemArgument.md). See that document for the full property list.

## XAML Examples

### UrlOrId Mode

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:models="clr-namespace:UiPath.GSuite.Activities.Tasks.Models;assembly=UiPath.GSuite.Activities"
-->
<models:TaskItemArgument InputMode="UrlOrId">
  <models:TaskItemArgument.ListId>
    <InArgument x:TypeArguments="x:String">list-id-123</InArgument>
  </models:TaskItemArgument.ListId>
  <models:TaskItemArgument.TaskId>
    <InArgument x:TypeArguments="x:String">task-id-456</InArgument>
  </models:TaskItemArgument.TaskId>
</models:TaskItemArgument>
```

### UseExisting Mode

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:models="clr-namespace:UiPath.GSuite.Activities.Tasks.Models;assembly=UiPath.GSuite.Activities"
    xmlns:tasks="clr-namespace:UiPath.GSuite.Activities.Tasks;assembly=UiPath.GSuite.Activities"
-->
<models:TaskItemArgument InputMode="UseExisting">
  <models:TaskItemArgument.ListOrTask>
    <InArgument x:TypeArguments="tasks:ITaskItem">[MyTaskVariable]</InArgument>
  </models:TaskItemArgument.ListOrTask>
</models:TaskItemArgument>
```

### FullPath Mode

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:models="clr-namespace:UiPath.GSuite.Activities.Tasks.Models;assembly=UiPath.GSuite.Activities"
-->
<models:TaskItemArgument InputMode="FullPath">
  <models:TaskItemArgument.FullPath>
    <InArgument x:TypeArguments="x:String">My Tasks/Buy groceries</InArgument>
  </models:TaskItemArgument.FullPath>
</models:TaskItemArgument>
```

## Notes

- Unlike [ListOrTaskItemArgument](ListOrTaskItemArgument.md), this component validates that **both** a list ID and a task ID are provided in `UrlOrId` mode, and that a `TaskBrowserId` is present in `Browse` mode.
- In `UseExisting` mode, the provided variable should be a [GTask](../types/GTask.md) (not a [GTaskList](../types/GTaskList.md)).

## Used By

Google Tasks activities that operate on a specific task -- see activity docs for details.
