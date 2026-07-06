# Download File

`UiPath.MicrosoftOffice365.Activities.Files.DownloadFileConnections`

Downloads a file from OneDrive or SharePoint to the local file system.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | File | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The file to download. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `DestinationPath` | Destination Path | `InArgument` | `String` | No | | The local path where the downloaded file will be saved. |
| `ConvertToPdf` | Convert to PDF | `InArgument` | `Boolean` | No | `False` | When true, converts the file to PDF during download. |
| `ConflictResolution` | Conflict Resolution | `InArgument` | `ConflictBehavior` | No | `Fail` | What to do when a local file with the same name exists: `Fail`, `Replace`, `Rename`. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `NewResult` | Result | `OutArgument` | `O365DriveLocalItem` | The downloaded file with full metadata. |

## XAML Example

```xml
<o365files:DownloadFileConnections DisplayName="Download File" ConnectionId="[conn]"
    DestinationPath="[localPath]">
    <o365files:DownloadFileConnections.Item>
        <models:DriveItemArgument ItemSelectionMode="ItemUrl">
            <models:DriveItemArgument.ManualEntryItemUrl>
                <InArgument x:TypeArguments="x:String">[fileUrl]</InArgument>
            </models:DriveItemArgument.ManualEntryItemUrl>
        </models:DriveItemArgument>
    </o365files:DownloadFileConnections.Item>
</o365files:DownloadFileConnections>
```
