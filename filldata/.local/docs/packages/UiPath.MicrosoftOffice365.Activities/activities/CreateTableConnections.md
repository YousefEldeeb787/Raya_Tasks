# Create Table

`UiPath.MicrosoftOffice365.Activities.Excel.CreateTableConnections`

Creates a table from a specified range in an Excel Online workbook.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Workbook | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The workbook containing the range. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `SheetName` | Sheet Name | `InArgument` | `String` | Yes | | The name of the sheet containing the range. |
| `Range` | Range | `InArgument` | `String` | Yes | | The cell range for the table (e.g., "A1:D10"). |
| `TableName` | Table Name | `InArgument` | `String` | Yes | | The name for the new table. |
| `HasHeaders` | Has Headers | `Property` | `Boolean` | No | `False` | Whether the first row of the range is used as the header row. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

## XAML Example

```xml
<o365excel:CreateTableConnections DisplayName="Create Table" ConnectionId="[conn]"
    SheetName="[sheetName]" Range="[range]" TableName="[tableName]" HasHeaders="True">
    <o365excel:CreateTableConnections.Item>
        <models:DriveItemArgument ItemSelectionMode="ItemUrl">
            <models:DriveItemArgument.ManualEntryItemUrl>
                <InArgument x:TypeArguments="x:String">[workbookUrl]</InArgument>
            </models:DriveItemArgument.ManualEntryItemUrl>
        </models:DriveItemArgument>
    </o365excel:CreateTableConnections.Item>
</o365excel:CreateTableConnections>
```
