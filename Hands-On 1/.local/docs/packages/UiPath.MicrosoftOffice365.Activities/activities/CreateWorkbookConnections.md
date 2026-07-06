# Create Workbook

`UiPath.MicrosoftOffice365.Activities.Excel.CreateWorkbookConnections`

Creates a new Excel workbook in a specified OneDrive or SharePoint folder.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Destination Folder | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The folder where the workbook will be created. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `WorkbookName` | Workbook Name | `InArgument` | `String` | Yes | | The file name for the new workbook. |
| `FirstSheetName` | First Sheet Name | `InArgument` | `String` | No | `Sheet1` | The name of the first sheet. |
| `ConflictResolution` | Conflict Resolution | `InArgument` | `ConflictBehavior` | No | `Replace` | What to do when a workbook with the same name exists: `Fail`, `Replace`, `Rename`. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | [`O365DriveRemoteItem`](types/O365DriveRemoteItem.md) | The newly created workbook. |

## XAML Example

```xml
<o365excel:CreateWorkbookConnections DisplayName="Create Workbook" ConnectionId="[conn]"
    WorkbookName="[name]" Result="[workbook]">
    <o365excel:CreateWorkbookConnections.Item>
        <models:DriveItemArgument ItemSelectionMode="ItemUrl">
            <models:DriveItemArgument.ManualEntryItemUrl>
                <InArgument x:TypeArguments="x:String">[folderUrl]</InArgument>
            </models:DriveItemArgument.ManualEntryItemUrl>
        </models:DriveItemArgument>
    </o365excel:CreateWorkbookConnections.Item>
</o365excel:CreateWorkbookConnections>
```
