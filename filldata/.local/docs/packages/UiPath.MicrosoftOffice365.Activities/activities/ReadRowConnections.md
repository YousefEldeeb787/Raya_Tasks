# Read Row

`UiPath.MicrosoftOffice365.Activities.Excel.ReadRowConnections`

Reads values from a row in an Excel Online workbook.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Workbook | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The workbook to read from. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `RangeLocation` | Range Location | `InArgument` | `String` | Yes | | The sheet name or named range containing the row. |
| `Cell` | Starting Cell | `InArgument` | `String` | Yes | | The starting cell of the row (e.g., "A1"). |
| `ValuesType` | Values Type | `InArgument` | `ValuesType` | No | `Values` | Whether to read values or formulas: `Values`, `Formulas`. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | `DataRow` | The row values as a DataRow. |

## XAML Example

```xml
<o365excel:ReadRowConnections DisplayName="Read Row" ConnectionId="[conn]"
    RangeLocation="[sheetName]" Cell="[startCell]" Result="[rowData]">
    <o365excel:ReadRowConnections.Item>
        <models:DriveItemArgument ItemSelectionMode="ItemUrl">
            <models:DriveItemArgument.ManualEntryItemUrl>
                <InArgument x:TypeArguments="x:String">[workbookUrl]</InArgument>
            </models:DriveItemArgument.ManualEntryItemUrl>
        </models:DriveItemArgument>
    </o365excel:ReadRowConnections.Item>
</o365excel:ReadRowConnections>
```
