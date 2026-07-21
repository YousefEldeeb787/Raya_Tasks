# GTaskList

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Tasks.Models.GTaskList`

Represents a Google Task list (a container for tasks).

**Assembly:** `UiPath.GSuite`
**Implements:** `ITaskItem`

## Properties

| Property | Type | Description |
|----------|------|-------------|
| `Title` | `string` | Title of the task list. |
| `Url` | `string` | URL pointing to this task list. |
| `Id` | `string` | The unique identifier of the task list. |
| `LastModified` | `DateTime?` | Last modification time of the task list. |

## Notes

- Implements the `ITaskItem` interface, which is shared with [GTask](GTask.md). This allows activities to accept either a task list or an individual task where appropriate.

## Used By

Activities that return or accept this type -- see activity docs for details. Also used by [ListOrTaskItemArgument](../components/ListOrTaskItemArgument.md) in `UseExisting` mode.
