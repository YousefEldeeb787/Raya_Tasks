# Copy Item

`UiPath.MicrosoftOffice365.Activities.Files.CopyItemConnections`

Copies a file or folder to the specified destination folder in OneDrive or SharePoint.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Item | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The source file or folder to copy. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `Folder` | Destination Folder | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The destination folder. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `NewName` | New Name | `InArgument` | `String` | No | | An optional new name for the copied item. If not specified, the original name is preserved. |
| `ConflictResolution` | Conflict Resolution | `InArgument` | `ConflictBehavior` | No | `Fail` | What to do when a file with the same name exists: `Fail`, `Replace`, `Rename`. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | [`O365DriveRemoteItem`](types/O365DriveRemoteItem.md) | The newly created copy of the file or folder. |

## XAML Example

```xml
<o365files:CopyItemConnections DisplayName="Copy File" ConnectionId="[conn]">
    <o365files:CopyItemConnections.Item>
        <models:DriveItemArgument ItemSelectionMode="ItemUrl">
            <models:DriveItemArgument.ManualEntryItemUrl>
                <InArgument x:TypeArguments="x:String">[sourceUrl]</InArgument>
            </models:DriveItemArgument.ManualEntryItemUrl>
        </models:DriveItemArgument>
    </o365files:CopyItemConnections.Item>
    <o365files:CopyItemConnections.Folder>
        <models:DriveItemArgument ItemSelectionMode="FullPath">
            <models:DriveItemArgument.ManualEntryItemFullPath>
                <InArgument x:TypeArguments="x:String">[destinationPath]</InArgument>
            </models:DriveItemArgument.ManualEntryItemFullPath>
        </models:DriveItemArgument>
    </o365files:CopyItemConnections.Folder>
</o365files:CopyItemConnections>
```
