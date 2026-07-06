# Download Email Attachments

`UiPath.MicrosoftOffice365.Activities.Mail.DownloadEmailAttachments`

Downloads the attachments from an email to a local folder.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Mail |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `Email` | Email | Input | [`InArgument<Office365Message>`](types/Office365Message.md) | Yes | | The email to get the attachments from. |
| `ExcludeInlineAttachments` | Exclude Inline Attachments | Options | `InArgument<bool>` | No | `False` | Indicates whether to exclude inline attachments embedded in the email body. |
| `FilterByFileNames` | Filter By File Names | Options | `InArgument<string>` | No | | Download only attachments whose name matches the specified pattern. Separate multiple patterns with a vertical bar. |
| `DestinationPath` | Destination Path | Options | `InArgument<string>` | No | | The path where to save the downloaded attachments. |
| `ConflictResolution` | Conflict Resolution | Options | `InArgument<ConflictBehavior>` | No | `Fail` | The conflict resolution behavior when a file with the same name exists. |

## Output Properties

| Property | Display Name | Category | Type | Description |
|---|---|---|---|---|
| `NewResult` | Attachments | Output | `OutArgument<O365AttachmentLocalItem[]>` | The downloaded attachment files. |

## Output Model

The `NewResult` property returns an array of `O365AttachmentLocalItem` representing the downloaded files.

## Enum Reference

| Enum | Values |
|---|---|
| `ConflictBehavior` | `Fail`, `Replace`, `Rename` |

## XAML Example

```xml
<mail:DownloadEmailAttachments
    DisplayName="Download Email Attachments"
    Email="{x:Null}"
    DestinationPath="C:\Downloads" />
```

## Notes

- The `Email` property is required.
- Supports both simple filename filtering and advanced condition-based filtering.
- If `DestinationPath` is provided, the files are saved to disk.
