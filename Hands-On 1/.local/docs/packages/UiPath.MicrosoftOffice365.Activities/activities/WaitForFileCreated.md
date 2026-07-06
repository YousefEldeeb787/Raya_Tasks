# Wait For File Created

`UiPath.MicrosoftOffice365.Activities.WaitForFileCreated`

Pauses the workflow and waits until a new file is created in the monitored OneDrive or SharePoint folder.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Folder | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The folder to monitor for new files. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `Filter` | Filter | `Property` | [`FileFolderFilterArgument`](filtering/FileFolderFilterArgument.md) | No | | Filter criteria for the created file. See [FileFolderFilterArgument](filtering/FileFolderFilterArgument.md). |
| `Timeout` | Timeout | `InArgument` | `TimeSpan` | No | | Maximum time to wait before timing out. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | [`O365DriveRemoteItem`](types/O365DriveRemoteItem.md) | The newly created file. |

## XAML Example

```xml
<o365files:WaitForFileCreated DisplayName="Wait For File Created" ConnectionId="[conn]"
    Result="[createdFile]">
    <o365files:WaitForFileCreated.Item>
        <models:DriveItemArgument ItemSelectionMode="ItemUrl">
            <models:DriveItemArgument.ManualEntryItemUrl>
                <InArgument x:TypeArguments="x:String">[folderUrl]</InArgument>
            </models:DriveItemArgument.ManualEntryItemUrl>
        </models:DriveItemArgument>
    </o365files:WaitForFileCreated.Item>
</o365files:WaitForFileCreated>
```
