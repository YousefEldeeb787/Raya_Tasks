# Task Created

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Activities.Tasks.Triggers.TaskCreated`

Trigger that fires when a new Google Task is created.

**Package:** `UiPath.GSuite.Activities`
**Category:** Tasks
**Connector:** `uipath-google-tasks`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `TaskList` | Task List | `Property` | [`ListOrTaskItemArgument`](components/ListOrTaskItemArgument.md) | No | | The task list to monitor for new tasks. See [ListOrTaskItemArgument](components/ListOrTaskItemArgument.md) for input modes. |
| `Filter` | Filter | `Property` | `TriggerTaskFilterCollection` | No | | Filter conditions. Supports: Title, Details, DueDate. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google Workspace connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Task | `OutArgument` | [`GTask`](types/GTask.md) | The newly created task. |
| `JobData` | Job Data | `OutArgument` | `JobInformation` | Details about the currently executing job. |

## Output Model

Returns a [`GTask`](types/GTask.md) representing the newly created task.

## XAML Example

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:gsuite="clr-namespace:UiPath.GSuite.Activities;assembly=UiPath.GSuite.Activities"
-->
<taskTriggers:TaskCreated
    DisplayName="Task Created"
    ConnectionId="{x:Null}"
    Result="[newTask]"
    JobData="[jobInfo]"
    xmlns:taskTriggers="clr-namespace:UiPath.GSuite.Activities.Tasks.Triggers;assembly=UiPath.GSuite.Activities" />
```

## Notes

- This is a trigger activity used in trigger-based workflows.
- The task list is selected via a [`ListOrTaskItemArgument`](components/ListOrTaskItemArgument.md) (`TaskList`), typically configured through the Studio designer.
- If no task list is specified, the trigger monitors all task lists.
