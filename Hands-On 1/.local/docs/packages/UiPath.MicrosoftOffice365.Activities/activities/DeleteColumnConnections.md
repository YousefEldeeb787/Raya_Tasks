# Delete Column

`UiPath.MicrosoftOffice365.Activities.Excel.DeleteColumnConnections`

Deletes a column from a range in an Excel Online workbook.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Workbook | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The workbook containing the column. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `RangeLocation` | Range Location | `InArgument` | `String` | Yes | | The sheet name or named range. |
| `Range` | Range | `InArgument` | `String` | Yes | | The range containing the column (e.g., "A1:D10"). |
| `Column` | Column | `InArgument` | `String` | Yes | | The column name or index to delete. |
| `HasHeaders` | Has Headers | `InArgument` | `Boolean` | No | `True` | Whether the range includes header rows. |
| `DeleteMode` | Delete Mode | `InArgument` | `ColumnDeleteMode` | No | `Delete` | How to delete the column: `Delete`, `Clear`. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `RangeInformation` | Range Information | `OutArgument` | `RangeInformation` | Updated range information after deletion. |
