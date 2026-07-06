# For Each Row

`UiPath.MicrosoftOffice365.Activities.Excel.ForEachRowConnections`

Iterates over each row in a specified Excel Online range or table.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Workbook | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The workbook containing the data. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `RangeLocation` | Range Location | `InArgument` | `String` | Yes | | The sheet name or named range containing the data. |
| `Range` | Range | `InArgument` | `String` | No | | The cell range to iterate over (e.g., "A1:D10"). If empty, the entire used range is used. |
| `HasHeaders` | Has Headers | `InArgument` | `Boolean` | No | `True` | Whether the first row contains column headers. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Length` | Length | `OutArgument` | `Int32` | The number of rows processed. |

### Body

Each iteration provides:
- `CurrentRow` (`DataRow`) -- the current row.
- `CurrentRowIndex` (`Int32`) -- the zero-based iteration index.

## XAML Example

```xml
<o365excel:ForEachRowConnections DisplayName="For Each Row" ConnectionId="[conn]"
    RangeLocation="[sheetName]" Range="[range]">
    <o365excel:ForEachRowConnections.Item>
        <models:DriveItemArgument ItemSelectionMode="ItemUrl">
            <models:DriveItemArgument.ManualEntryItemUrl>
                <InArgument x:TypeArguments="x:String">[workbookUrl]</InArgument>
            </models:DriveItemArgument.ManualEntryItemUrl>
        </models:DriveItemArgument>
    </o365excel:ForEachRowConnections.Item>
    <ActivityAction x:TypeArguments="sd:DataRow, x:Int32">
        <ActivityAction.Argument1><DelegateInArgument x:TypeArguments="sd:DataRow" Name="CurrentRow" /></ActivityAction.Argument1>
        <ActivityAction.Argument2><DelegateInArgument x:TypeArguments="x:Int32" Name="CurrentRowIndex" /></ActivityAction.Argument2>
        <Sequence DisplayName="Do" />
    </ActivityAction>
</o365excel:ForEachRowConnections>
```
