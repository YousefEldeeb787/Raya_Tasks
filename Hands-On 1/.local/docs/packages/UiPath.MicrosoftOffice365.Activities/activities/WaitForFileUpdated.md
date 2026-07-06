# Wait For File Updated

`UiPath.MicrosoftOffice365.Activities.WaitForFileUpdated`

Pauses the workflow and waits until a file is updated in the monitored OneDrive or SharePoint folder.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Folder | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The folder to monitor for file updates. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `Filter` | Filter | `Property` | [`FileFolderFilterArgument`](filtering/FileFolderFilterArgument.md) | No | | Filter criteria for the updated file. See [FileFolderFilterArgument](filtering/FileFolderFilterArgument.md). |
| `Timeout` | Timeout | `InArgument` | `TimeSpan` | No | | Maximum time to wait before timing out. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | [`O365DriveRemoteItem`](types/O365DriveRemoteItem.md) | The updated file. |

## XAML Example

```xml
<o365files:WaitForFileUpdated DisplayName="Wait For File Updated" ConnectionId="[conn]"
    Result="[updatedFile]">
    <o365files:WaitForFileUpdated.Item>
        <models:DriveItemArgument ItemSelectionMode="ItemUrl">
            <models:DriveItemArgument.ManualEntryItemUrl>
                <InArgument x:TypeArguments="x:String">[folderUrl]</InArgument>
            </models:DriveItemArgument.ManualEntryItemUrl>
        </models:DriveItemArgument>
    </o365files:WaitForFileUpdated.Item>
</o365files:WaitForFileUpdated>
```
