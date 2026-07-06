# For Each Sheet

`UiPath.MicrosoftOffice365.Activities.Excel.ForEachSheetConnections`

Iterates over all sheets in an Excel Online workbook.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Workbook | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The workbook to iterate sheets from. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Length` | Length | `OutArgument` | `Int32` | The number of sheets processed. |

### Body

Each iteration provides:
- `CurrentItem` (`String`) -- the current sheet name.
- `CurrentItemIndex` (`Int32`) -- the zero-based iteration index.

## XAML Example

```xml
<o365excel:ForEachSheetConnections DisplayName="For Each Sheet" ConnectionId="[conn]">
    <o365excel:ForEachSheetConnections.Item>
        <models:DriveItemArgument ItemSelectionMode="ItemUrl">
            <models:DriveItemArgument.ManualEntryItemUrl>
                <InArgument x:TypeArguments="x:String">[workbookUrl]</InArgument>
            </models:DriveItemArgument.ManualEntryItemUrl>
        </models:DriveItemArgument>
    </o365excel:ForEachSheetConnections.Item>
    <ActivityAction x:TypeArguments="x:String, x:Int32">
        <ActivityAction.Argument1><DelegateInArgument x:TypeArguments="x:String" Name="CurrentItem" /></ActivityAction.Argument1>
        <ActivityAction.Argument2><DelegateInArgument x:TypeArguments="x:Int32" Name="CurrentItemIndex" /></ActivityAction.Argument2>
        <Sequence DisplayName="Do" />
    </ActivityAction>
</o365excel:ForEachSheetConnections>
```
