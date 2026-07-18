# Wait For Task Completed and Resume

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Activities.WaitForTaskCompleted`

Pauses the workflow and resumes when a Google Task is marked as completed.

**Package:** `UiPath.GSuite.Activities`
**Category:** Tasks
**Connector:** `uipath-google-tasks`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `ListItemArgument` | Task List | `Property` | [`ListOrTaskItemArgument`](components/ListOrTaskItemArgument.md) | Yes | | The task list to monitor for completed tasks. See [ListOrTaskItemArgument](components/ListOrTaskItemArgument.md) for input modes. |
| `Filter` | Filter | `Property` | `TriggerTaskFilterWithVariablesCollection` | No | | Filter conditions to match completed tasks. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google Workspace connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Task | `OutArgument` | [`GTask`](types/GTask.md) | The completed task. |
| `JobData` | Job Data | `OutArgument` | `JobInformation` | Details about the currently executing job. |

## Output Model

Returns a [`GTask`](types/GTask.md) representing the task that was completed.

## XAML Example

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:gsuite="clr-namespace:UiPath.GSuite.Activities;assembly=UiPath.GSuite.Activities"
    xmlns:models="clr-namespace:UiPath.GSuite.Activities.Tasks.Models;assembly=UiPath.GSuite.Activities"
-->
<gsuite:WaitForTaskCompleted
    DisplayName="Wait for Task Completed"
    ConnectionId="{x:Null}"
    Result="[completedTask]"
    JobData="[jobInfo]">
    <gsuite:WaitForTaskCompleted.ListItemArgument>
        <models:ListOrTaskItemArgument InputMode="UrlOrId">
            <models:ListOrTaskItemArgument.ListId>
                <InArgument x:TypeArguments="x:String">[taskListId]</InArgument>
            </models:ListOrTaskItemArgument.ListId>
        </models:ListOrTaskItemArgument>
    </gsuite:WaitForTaskCompleted.ListItemArgument>
</gsuite:WaitForTaskCompleted>
```

## Notes

- This is a persistence activity -- the workflow suspends and resumes when the trigger condition is met.
- The activity first creates an inbox subscription, then persists. On resume, it retrieves the completed task details.
- In debug mode, the activity attempts to find a sample matching completed task instead of persisting.
