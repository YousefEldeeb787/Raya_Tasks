# Form Response Updated

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Activities.FormResponseUpdated`

Trigger that fires when an existing Google Form response is updated.

**Package:** `UiPath.GSuite.Activities`
**Category:** Forms
**Connector:** `uipath-google-forms`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Form` | Form | `Property` | `TriggerDriveItemArgument` | Yes | | The Google Form to monitor for updated responses. Configured through the Studio designer. |
| `Filter` | Filter | `Property` | `TriggerFormResponseFilterCollection` | No | | Filter conditions applied to form responses; only matching responses activate the trigger. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google Workspace connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Response | `OutArgument` | `GFormResponse` | The updated form response that triggered the workflow. |
| `JobData` | Job Data | `OutArgument` | `JobInformation` | Details about the currently executing job. |

## XAML Example

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:gsuite="clr-namespace:UiPath.GSuite.Activities;assembly=UiPath.GSuite.Activities"
-->
<gsuite:FormResponseUpdated
    DisplayName="Form Response Updated"
    ConnectionId="{x:Null}"
    Result="[updatedResponse]"
    JobData="[jobInfo]"
    xmlns:gsuite="clr-namespace:UiPath.GSuite.Activities;assembly=UiPath.GSuite.Activities" />
```

## Notes

- This is a trigger activity used in trigger-based workflows.
- The form is selected via a `TriggerDriveItemArgument`, typically configured through the Studio designer.
- Requires one of the following OAuth scopes: `drive`, `drive.file`, or `forms.responses.readonly`.
