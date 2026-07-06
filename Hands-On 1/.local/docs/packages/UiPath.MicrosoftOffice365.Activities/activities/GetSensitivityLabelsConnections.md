# Get Sensitivity Labels

`UiPath.MicrosoftOffice365.Activities.Files.GetSensitivityLabelsConnections`

Gets the sensitivity labels assigned to a specified file. This is a Files activity (OneDrive connector), not a Mail activity.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Files |
| **Connector** | `uipath-microsoft-onedrive` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `Item` | File | Input | `InArgument<O365DriveRemoteItem>` | Yes | | The file for which to extract the sensitivity labels. |

## Output Properties

| Property | Display Name | Category | Type | Description |
|---|---|---|---|---|
| `SensitivityLabels` | Sensitivity Labels | Output | `OutArgument<IEnumerable<SensitivityLabel>>` | The list of sensitivity labels assigned to the specified file. |

## Output Model

Each `SensitivityLabel` represents a sensitivity label with its properties.

## XAML Example

```xml
<files:GetSensitivityLabelsConnections
    DisplayName="Get Sensitivity Labels"
    Item="{x:Null}" />
```

## Notes

- The `Item` property is required and must be a file (not a folder).
- This activity uses the OneDrive connector, not the Outlook connector.
