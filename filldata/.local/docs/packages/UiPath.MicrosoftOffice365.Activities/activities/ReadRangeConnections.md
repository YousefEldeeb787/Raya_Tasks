# Read Range

`UiPath.MicrosoftOffice365.Activities.Excel.ReadRangeConnections`

Reads data from a range in an Excel Online workbook and returns it as a DataTable.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Workbook | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The workbook to read from. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `RangeLocation` | Range Location | `InArgument` | `String` | Yes | | The sheet name or named range containing the data. |
| `Range` | Range | `InArgument` | `String` | No | | The cell range to read (e.g., "A1:D10"). If empty, the entire used range is used. |
| `HasHeaders` | Has Headers | `InArgument` | `Boolean` | No | `True` | Whether the first row contains column headers. |
| `WhatToRead` | What to Read | `Property` | `ValuesType` | No | `Values` | Whether to read values, formulas, or formatted text: `Values`, `Formulas`, `Text`. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | `DataTable` | The range data as a DataTable. |

## XAML Example

```xml
<o365excel:ReadRangeConnections DisplayName="Read Range" ConnectionId="[conn]"
    RangeLocation="[sheetName]" Range="[range]" Result="[dataTable]">
    <o365excel:ReadRangeConnections.Item>
        <models:DriveItemArgument ItemSelectionMode="ItemUrl">
            <models:DriveItemArgument.ManualEntryItemUrl>
                <InArgument x:TypeArguments="x:String">[workbookUrl]</InArgument>
            </models:DriveItemArgument.ManualEntryItemUrl>
        </models:DriveItemArgument>
    </o365excel:ReadRangeConnections.Item>
</o365excel:ReadRangeConnections>
```
