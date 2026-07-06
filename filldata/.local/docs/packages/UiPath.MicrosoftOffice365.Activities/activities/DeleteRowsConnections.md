# Delete Rows

`UiPath.MicrosoftOffice365.Activities.Excel.DeleteRowsConnections`

Deletes rows from a range in an Excel Online workbook.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Workbook | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The workbook containing the rows. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `RangeLocation` | Range Location | `InArgument` | `String` | Yes | | The sheet name or named range. |
| `Range` | Range | `InArgument` | `String` | Yes | | The range containing the rows (e.g., "A1:D10"). |
| `Rows` | Rows | `InArgument` | `String` | Yes | | The row ranges to delete (e.g., "1,3-5"). |
| `DeleteMode` | Delete Mode | `InArgument` | `RowsDeleteMode` | No | `Clear` | How to delete: `Clear`, `ShiftUp`, `Delete`. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `RangeInformation` | Range Information | `OutArgument` | `RangeInformation` | Updated range information after deletion. |
