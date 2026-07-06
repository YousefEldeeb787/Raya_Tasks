# VLookup Range

`UiPath.MicrosoftOffice365.Activities.Excel.VLookupRangeConnections`

Performs a VLookup operation on a specified range in an Excel Online workbook.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Workbook | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The workbook containing the range. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `RangeLocation` | Range Location | `InArgument` | `String` | Yes | | The sheet name or named range. |
| `Range` | Range | `InArgument` | `String` | Yes | | The range to look up in (e.g., "A1:D10"). |
| `ValueToLookup` | Value to Lookup | `InArgument` | `Object` | Yes | | The value to search for in the first column. |
| `ColumnIndex` | Column Index | `InArgument` | `Int32` | Yes | | The column index containing the return value. |
| `ExactMatch` | Exact Match | `InArgument` | `Boolean` | No | `True` | Whether to require an exact match. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | `String` | The lookup result value. |
