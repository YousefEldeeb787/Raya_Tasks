# Move Item

`UiPath.MicrosoftOffice365.Activities.Files.MoveItemConnections`

Moves a file or folder to the specified destination folder in OneDrive or SharePoint.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Item | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The source file or folder to move. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `Folder` | Destination Folder | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The destination folder. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `NewName` | New Name | `InArgument` | `String` | No | | An optional new name for the moved item. If not specified, the original name is preserved. |
| `ConflictResolution` | Conflict Resolution | `InArgument` | `ConflictBehavior` | No | `Fail` | What to do when a file with the same name exists: `Fail`, `Replace`, `Rename`. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | [`O365DriveRemoteItem`](types/O365DriveRemoteItem.md) | The moved file or folder at its new location. |

## XAML Example

```xml
<o365files:MoveItemConnections DisplayName="Move Item" ConnectionId="[conn]">
    <o365files:MoveItemConnections.Item>
        <models:DriveItemArgument ItemSelectionMode="UseExisting">
            <models:DriveItemArgument.Item>
                <InArgument x:TypeArguments="files:O365DriveRemoteItem">[sourceItem]</InArgument>
            </models:DriveItemArgument.Item>
        </models:DriveItemArgument>
    </o365files:MoveItemConnections.Item>
    <o365files:MoveItemConnections.Folder>
        <models:DriveItemArgument ItemSelectionMode="ItemUrl">
            <models:DriveItemArgument.ManualEntryItemUrl>
                <InArgument x:TypeArguments="x:String">[destinationFolderUrl]</InArgument>
            </models:DriveItemArgument.ManualEntryItemUrl>
        </models:DriveItemArgument>
    </o365files:MoveItemConnections.Folder>
</o365files:MoveItemConnections>
```
