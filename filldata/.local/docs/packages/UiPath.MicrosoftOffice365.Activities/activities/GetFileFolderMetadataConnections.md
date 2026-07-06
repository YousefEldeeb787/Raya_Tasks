# Get File/Folder Metadata

`UiPath.MicrosoftOffice365.Activities.Files.GetFileFolderMetadataConnections`

Retrieves SharePoint metadata (column values) for a file or folder.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | File or Folder | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The file or folder whose metadata to retrieve. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `ColumnsToRetrieveByName` | Columns to Retrieve | `InArgument` | `IEnumerable<Object>` | Yes | | The SharePoint list columns to retrieve, specified by name. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | `DataRow` | The metadata values as a DataRow. |

## XAML Example

```xml
<o365files:GetFileFolderMetadataConnections DisplayName="Get Metadata" ConnectionId="[conn]"
    Result="[metadataRow]">
    <o365files:GetFileFolderMetadataConnections.Item>
        <models:DriveItemArgument ItemSelectionMode="ItemUrl">
            <models:DriveItemArgument.ManualEntryItemUrl>
                <InArgument x:TypeArguments="x:String">[fileUrl]</InArgument>
            </models:DriveItemArgument.ManualEntryItemUrl>
        </models:DriveItemArgument>
    </o365files:GetFileFolderMetadataConnections.Item>
</o365files:GetFileFolderMetadataConnections>
```
