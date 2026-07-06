# Delete Item

`UiPath.MicrosoftOffice365.Activities.Files.DeleteItemConnections`

Deletes a file or folder from OneDrive or SharePoint.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Item | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The file or folder to delete. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `PermanentlyDelete` | Permanently Delete | `InArgument` | `Boolean` | No | `False` | When true, the item is permanently deleted. When false, moved to Recycle Bin. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

## XAML Example

```xml
<o365files:DeleteItemConnections DisplayName="Delete Item" ConnectionId="[conn]">
    <o365files:DeleteItemConnections.Item>
        <models:DriveItemArgument ItemSelectionMode="UseExisting">
            <models:DriveItemArgument.Item>
                <InArgument x:TypeArguments="files:O365DriveRemoteItem">[fileToDelete]</InArgument>
            </models:DriveItemArgument.Item>
        </models:DriveItemArgument>
    </o365files:DeleteItemConnections.Item>
</o365files:DeleteItemConnections>
```
