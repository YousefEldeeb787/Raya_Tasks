# Rename Sheet

`UiPath.MicrosoftOffice365.Activities.Excel.RenameSheetConnections`

Renames a sheet in an Excel Online workbook.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Workbook | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The workbook containing the sheet to rename. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `OldSheetName` | Old Sheet Name | `InArgument` | `String` | Yes | | The current name of the sheet. |
| `NewSheetName` | New Sheet Name | `InArgument` | `String` | Yes | | The new name for the sheet. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

## XAML Example

```xml
<o365excel:RenameSheetConnections DisplayName="Rename Sheet" ConnectionId="[conn]"
    OldSheetName="[oldName]" NewSheetName="[newName]">
    <o365excel:RenameSheetConnections.Item>
        <models:DriveItemArgument ItemSelectionMode="ItemUrl">
            <models:DriveItemArgument.ManualEntryItemUrl>
                <InArgument x:TypeArguments="x:String">[workbookUrl]</InArgument>
            </models:DriveItemArgument.ManualEntryItemUrl>
        </models:DriveItemArgument>
    </o365excel:RenameSheetConnections.Item>
</o365excel:RenameSheetConnections>
```
