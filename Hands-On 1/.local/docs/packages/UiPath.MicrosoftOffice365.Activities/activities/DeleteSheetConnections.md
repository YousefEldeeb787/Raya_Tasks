# Delete Sheet

`UiPath.MicrosoftOffice365.Activities.Excel.DeleteSheetConnections`

Deletes a sheet from an Excel Online workbook.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Workbook | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The workbook containing the sheet to delete. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `SheetName` | Sheet Name | `InArgument` | `String` | Yes | | The name of the sheet to delete. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

## XAML Example

```xml
<o365excel:DeleteSheetConnections DisplayName="Delete Sheet" ConnectionId="[conn]"
    SheetName="[sheetName]">
    <o365excel:DeleteSheetConnections.Item>
        <models:DriveItemArgument ItemSelectionMode="ItemUrl">
            <models:DriveItemArgument.ManualEntryItemUrl>
                <InArgument x:TypeArguments="x:String">[workbookUrl]</InArgument>
            </models:DriveItemArgument.ManualEntryItemUrl>
        </models:DriveItemArgument>
    </o365excel:DeleteSheetConnections.Item>
</o365excel:DeleteSheetConnections>
```
