# Assign Sensitivity Label

`UiPath.MicrosoftOffice365.Activities.Files.AssignSensitivityLabelConnections`

Assigns, removes, or changes a sensitivity label on a file in OneDrive or SharePoint.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | File | `InArgument` | [`O365DriveRemoteItem`](types/O365DriveRemoteItem.md) | Yes | | The file to assign the sensitivity label to. |
| `ActionType` | Action Type | `InArgument` | `SensitivityLabelActionType` | Yes | `Add` | The action: `Add`, `Update`, `Clear`. |
| `SensitivityLabel` | Sensitivity Label | `InArgument` | `String` | When not Clear | | The sensitivity label ID to assign. |
| `Justification` | Justification | `InArgument` | `String` | When downgrading/removing | | Justification text for audit purposes. Required when downgrading or removing a label. |
| `WaitActionToComplete` | Wait for Completion | `InArgument` | `Boolean` | No | `False` | Whether to wait for the sensitivity label action to complete. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

## XAML Example

```xml
<o365files:AssignSensitivityLabelConnections DisplayName="Assign Label" ConnectionId="[conn]"
    ActionType="[SensitivityLabelActionType.Add]" SensitivityLabel="[labelId]">
    <o365files:AssignSensitivityLabelConnections.Item>
        <InArgument x:TypeArguments="files:O365DriveRemoteItem">[file]</InArgument>
    </o365files:AssignSensitivityLabelConnections.Item>
</o365files:AssignSensitivityLabelConnections>
```
