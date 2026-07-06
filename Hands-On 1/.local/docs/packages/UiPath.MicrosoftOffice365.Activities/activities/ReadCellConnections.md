# Read Cell

`UiPath.MicrosoftOffice365.Activities.Excel.ReadCellConnections`

Reads the value of a single cell from an Excel Online workbook.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Workbook | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The workbook to read from. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `RangeLocation` | Range Location | `InArgument` | `String` | Yes | | The sheet name or named range that contains the cell. |
| `Cell` | Cell | `InArgument` | `String` | Yes | | The cell address to read (e.g., "A1"). |
| `WhatToRead` | What to Read | `Property` | `ValuesType` | No | `Values` | Whether to read values, formulas, or formatted text: `Values`, `Formulas`, `Text`. |
| `CellValueType` | Cell Value Type | `Property` | `CellFormatType` | No | `Text` | The format type to apply: `Text`, `Number`, `Date`. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | `Object` | The value of the cell. |

## XAML Example

```xml
<o365excel:ReadCellConnections DisplayName="Read Cell" ConnectionId="[conn]"
    RangeLocation="[sheetName]" Cell="[cellAddress]" Result="[cellValue]">
    <o365excel:ReadCellConnections.Item>
        <models:DriveItemArgument ItemSelectionMode="ItemUrl">
            <models:DriveItemArgument.ManualEntryItemUrl>
                <InArgument x:TypeArguments="x:String">[workbookUrl]</InArgument>
            </models:DriveItemArgument.ManualEntryItemUrl>
        </models:DriveItemArgument>
    </o365excel:ReadCellConnections.Item>
</o365excel:ReadCellConnections>
```
