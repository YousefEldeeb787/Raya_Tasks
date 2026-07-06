# Create Folder

`UiPath.MicrosoftOffice365.Activities.Files.CreateFolderConnections`

Creates a folder at the specified location in OneDrive or SharePoint.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Parent Folder | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The parent folder where the new folder will be created. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `NewFolderName` | New Folder Name | `InArgument` | `String` | Yes | | The name to assign to the newly created folder. |
| `ConflictResolution` | Conflict Resolution | `InArgument` | `ConflictBehavior` | No | `Fail` | What to do when a folder with the same name exists: `Fail`, `Replace`, `Rename`. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | [`O365DriveRemoteItem`](types/O365DriveRemoteItem.md) | The newly created folder. |

## XAML Example

```xml
<o365files:CreateFolderConnections DisplayName="Create Folder" ConnectionId="[conn]"
    NewFolderName="[folderName]">
    <o365files:CreateFolderConnections.Item>
        <models:DriveItemArgument ItemSelectionMode="ItemUrl">
            <models:DriveItemArgument.ManualEntryItemUrl>
                <InArgument x:TypeArguments="x:String">[parentFolderUrl]</InArgument>
            </models:DriveItemArgument.ManualEntryItemUrl>
        </models:DriveItemArgument>
    </o365files:CreateFolderConnections.Item>
</o365files:CreateFolderConnections>
```
