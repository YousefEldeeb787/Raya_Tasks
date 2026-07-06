# Update File/Folder Metadata

`UiPath.MicrosoftOffice365.Activities.Files.UpdateFileFolderMetadataConnections`

Updates SharePoint metadata (column values) for a file or folder.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | File or Folder | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The file or folder whose metadata to update. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `DataRow` | Data Row | `InArgument` | `DataRow` | No | | A DataRow containing the field values to update. Used when columns are specified dynamically. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | `DataRow` | The updated metadata values as a DataRow. |

## XAML Example

```xml
<o365files:UpdateFileFolderMetadataConnections DisplayName="Update Metadata" ConnectionId="[conn]"
    DataRow="[dataRow]" Result="[updatedRow]">
    <o365files:UpdateFileFolderMetadataConnections.Item>
        <models:DriveItemArgument ItemSelectionMode="ItemUrl">
            <models:DriveItemArgument.ManualEntryItemUrl>
                <InArgument x:TypeArguments="x:String">[fileUrl]</InArgument>
            </models:DriveItemArgument.ManualEntryItemUrl>
        </models:DriveItemArgument>
    </o365files:UpdateFileFolderMetadataConnections.Item>
</o365files:UpdateFileFolderMetadataConnections>
```
