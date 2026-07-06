# Add Sheet

`UiPath.MicrosoftOffice365.Activities.Excel.AddSheetConnections`

Adds a new sheet to an Excel Online workbook.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Workbook | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The workbook to add the sheet to. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `SheetName` | Sheet Name | `InArgument` | `String` | No | | The name for the new sheet. If blank, Excel determines the name. |
| `ConflictResolution` | Conflict Resolution | `InArgument` | `ConflictBehavior` | No | `Fail` | What to do when a sheet with the same name exists: `Fail`, `Replace`, `Rename`. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `NewSheetName` | New Sheet Name | `OutArgument` | `String` | The resulting name of the newly added sheet. |

## XAML Example

```xml
<o365excel:AddSheetConnections DisplayName="Add Sheet" ConnectionId="[conn]"
    SheetName="[sheetName]" NewSheetName="[resultName]">
    <o365excel:AddSheetConnections.Item>
        <models:DriveItemArgument ItemSelectionMode="ItemUrl">
            <models:DriveItemArgument.ManualEntryItemUrl>
                <InArgument x:TypeArguments="x:String">[workbookUrl]</InArgument>
            </models:DriveItemArgument.ManualEntryItemUrl>
        </models:DriveItemArgument>
    </o365excel:AddSheetConnections.Item>
</o365excel:AddSheetConnections>
```
