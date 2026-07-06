# Upload Files

`UiPath.MicrosoftOffice365.Activities.Files.UploadFilesConnections`

Uploads files to a specified folder in OneDrive or SharePoint.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Destination Folder | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The folder to upload files to. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `FilesInputMode` | Files Input Mode | `Property` | `FilesInputMode` | No | `MultipleByVariable` | How to specify the files: `Single`, `MultipleByVariable`, `MultipleByBuilder`. |
| `MultipleFilesToUpload` | Files | `InArgument` | `IEnumerable<IResource>` | When MultipleByVariable | | A collection of file resources to upload. |
| `Metadata` | Metadata | `InArgument` | `DataTable` | No | | SharePoint metadata to upload along with the files. |
| `ConflictResolution` | Conflict Resolution | `InArgument` | `ConflictBehavior` | No | `Replace` | What to do when a file with the same name exists: `Fail`, `Replace`, `Rename`. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `FirstResult` | First Result | `OutArgument` | [`O365DriveRemoteItem`](types/O365DriveRemoteItem.md) | The first uploaded file, or null if none were uploaded. |
| `AllResults` | All Results | `OutArgument` | [`O365DriveRemoteItem`](types/O365DriveRemoteItem.md)`[]` | All uploaded files. |

## XAML Example

```xml
<o365files:UploadFilesConnections DisplayName="Upload Files" ConnectionId="[conn]"
    FilesInputMode="MultipleByVariable" FirstResult="[firstFile]" AllResults="[allFiles]">
    <o365files:UploadFilesConnections.Item>
        <models:DriveItemArgument ItemSelectionMode="ItemUrl">
            <models:DriveItemArgument.ManualEntryItemUrl>
                <InArgument x:TypeArguments="x:String">[destinationFolderUrl]</InArgument>
            </models:DriveItemArgument.ManualEntryItemUrl>
        </models:DriveItemArgument>
    </o365files:UploadFilesConnections.Item>
    <o365files:UploadFilesConnections.MultipleFilesToUpload>
        <InArgument x:TypeArguments="scg:IEnumerable(platform:IResource)">[filesToUpload]</InArgument>
    </o365files:UploadFilesConnections.MultipleFilesToUpload>
</o365files:UploadFilesConnections>
```
