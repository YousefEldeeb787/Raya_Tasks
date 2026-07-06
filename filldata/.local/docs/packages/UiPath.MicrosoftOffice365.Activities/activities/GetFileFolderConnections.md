# Get File/Folder

`UiPath.MicrosoftOffice365.Activities.Files.GetFileFolderConnections`

Gets a file or folder reference from OneDrive or SharePoint to use in other activities.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | File or Folder | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The file or folder to retrieve. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | [`O365DriveRemoteItem`](types/O365DriveRemoteItem.md) | The retrieved file or folder. |

## XAML Example

```xml
<o365files:GetFileFolderConnections DisplayName="Get File" ConnectionId="[conn]"
    Result="[fileResult]">
    <o365files:GetFileFolderConnections.Item>
        <models:DriveItemArgument ItemSelectionMode="ItemUrl">
            <models:DriveItemArgument.ManualEntryItemUrl>
                <InArgument x:TypeArguments="x:String">[fileUrl]</InArgument>
            </models:DriveItemArgument.ManualEntryItemUrl>
        </models:DriveItemArgument>
    </o365files:GetFileFolderConnections.Item>
</o365files:GetFileFolderConnections>
```
