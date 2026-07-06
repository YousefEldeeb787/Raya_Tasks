# Read Column

`UiPath.MicrosoftOffice365.Activities.Excel.ReadColumnConnections`

Reads values from a column in an Excel Online workbook.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Workbook | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The workbook to read from. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `RangeLocation` | Range Location | `InArgument` | `String` | Yes | | The sheet name or named range containing the column. |
| `Cell` | Starting Cell | `InArgument` | `String` | Yes | | The starting cell of the column (e.g., "A1"). |
| `ValuesType` | Values Type | `InArgument` | `ValuesType` | No | `Values` | Whether to read values or formulas: `Values`, `Formulas`. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | `Object[]` | The column values as an array. |

## XAML Example

```xml
<o365excel:ReadColumnConnections DisplayName="Read Column" ConnectionId="[conn]"
    RangeLocation="[sheetName]" Cell="[startCell]" Result="[columnValues]">
    <o365excel:ReadColumnConnections.Item>
        <models:DriveItemArgument ItemSelectionMode="ItemUrl">
            <models:DriveItemArgument.ManualEntryItemUrl>
                <InArgument x:TypeArguments="x:String">[workbookUrl]</InArgument>
            </models:DriveItemArgument.ManualEntryItemUrl>
        </models:DriveItemArgument>
    </o365excel:ReadColumnConnections.Item>
</o365excel:ReadColumnConnections>
```
