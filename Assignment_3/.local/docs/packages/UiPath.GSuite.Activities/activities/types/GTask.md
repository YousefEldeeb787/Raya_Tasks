# GTask

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Tasks.Models.GTask`

Represents a Google Task item within a task list.

**Assembly:** `UiPath.GSuite`
**Implements:** `ITaskItem`

## Properties

| Property | Type | Description |
|----------|------|-------------|
| `Title` | `string` | Title of the task. |
| `Details` | `string` | Notes describing the task. Optional. |
| `Due` | `DateTime?` | Due date of the task (as RFC 3339 timestamp). The time portion is discarded -- only the date is meaningful. |
| `Status` | `string` | Status of the task. Either `"needsAction"` or `"completed"`. |
| `ParentListId` | `string` | Parent list identifier. |
| `LastModified` | `DateTime?` | Last modification time of the task (as RFC 3339 timestamp). |
| `ParentId` | `string` | Parent task identifier. Omitted for top-level tasks. Read-only. |
| `SubTaskIds` | `IEnumerable<string>` | The list of subtask IDs. |
| `Url` | `string` | URL pointing to this task. Used to retrieve, update, or delete this task. |
| `Id` | `string` | Task identifier. |
| `Completed` | `DateTime?` | Completion date of the task (as RFC 3339 timestamp). Omitted if the task has not been completed. |
| `Deleted` | `bool?` | Flag indicating whether the task has been deleted. Default is False. |
| `Hidden` | `bool?` | Flag indicating whether the task is hidden. This is the case if the task had been marked completed when the task list was last cleared. Default is False. Read-only. |
| `Position` | `string` | String indicating the position of the task among its sibling tasks. Lexicographic ordering determines display order. Read-only. |

## Notes

- The `Due` property only records date information; the time portion is discarded by the Google Tasks API.
- Use the "move" operation to reposition a task rather than setting `Position` directly.
- `ParentId` is only present for subtasks; top-level tasks omit this field.

## Used By

Activities that return or accept this type -- see activity docs for details. Also used by [ListOrTaskItemArgument](../components/ListOrTaskItemArgument.md) and [TaskItemArgument](../components/TaskItemArgument.md) in `UseExisting` mode.
