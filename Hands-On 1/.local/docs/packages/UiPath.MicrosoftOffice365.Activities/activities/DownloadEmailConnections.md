# Download Email

`UiPath.MicrosoftOffice365.Activities.Mail.DownloadEmailConnections`

Downloads an email in EML file format to a local folder.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Mail |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `Email` | Email | Input | [`InArgument<Office365Message>`](types/Office365Message.md) | Yes | | The email to download. |
| `DestinationPath` | Destination Path | Options | `InArgument<string>` | No | | The path where to save the downloaded mail. |
| `CreateMissingFolders` | Create Missing Folders | | `InArgument<bool>` | No | `False` | Indicates whether to create missing folders in the destination path. |
| `ConflictResolution` | Conflict Resolution | Options | `InArgument<ConflictBehavior>` | No | `Fail` | The conflict resolution behavior when a file with the same name exists. |

## Output Properties

| Property | Display Name | Category | Type | Description |
|---|---|---|---|---|
| `NewResult` | Downloaded File | Output | `OutArgument<O365MailLocalItem>` | The downloaded EML file. |

## Output Model

The `NewResult` property returns an `O365MailLocalItem` representing the downloaded EML file.

## Enum Reference

| Enum | Values |
|---|---|
| `ConflictBehavior` | `Fail`, `Replace`, `Rename` |

## XAML Example

```xml
<mail:DownloadEmailConnections
    DisplayName="Download Email"
    Email="{x:Null}"
    DestinationPath="C:\Downloads" />
```

## Notes

- The `Email` property is required.
- If `DestinationPath` is provided, the file is saved to disk.
