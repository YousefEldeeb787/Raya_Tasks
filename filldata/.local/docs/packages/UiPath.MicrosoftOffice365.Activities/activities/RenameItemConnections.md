# Rename Item

`UiPath.MicrosoftOffice365.Activities.Files.RenameItemConnections`

Renames a file or folder in OneDrive or SharePoint.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Item | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The file or folder to rename. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `NewName` | New Name | `InArgument` | `String` | Yes | | The new name for the file or folder. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | [`O365DriveRemoteItem`](types/O365DriveRemoteItem.md) | The renamed file or folder. |

## XAML Example

```xml
<o365files:RenameItemConnections DisplayName="Rename Item" ConnectionId="[conn]"
    NewName="[newName]" Result="[renamedItem]">
    <o365files:RenameItemConnections.Item>
        <models:DriveItemArgument ItemSelectionMode="ItemUrl">
            <models:DriveItemArgument.ManualEntryItemUrl>
                <InArgument x:TypeArguments="x:String">[fileUrl]</InArgument>
            </models:DriveItemArgument.ManualEntryItemUrl>
        </models:DriveItemArgument>
    </o365files:RenameItemConnections.Item>
</o365files:RenameItemConnections>
```
