# Write Range

`UiPath.MicrosoftOffice365.Activities.Excel.WriteRangeConnections`

Writes a DataTable to a range in an Excel Online workbook.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Workbook | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The workbook to write to. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `RangeLocation` | Range Location | `InArgument` | `String` | Yes | | The sheet name or named range. |
| `Range` | Range | `InArgument` | `String` | Yes | | The starting cell for the write (e.g., "A1"). |
| `DataTable` | Data Table | `InArgument` | `DataTable` | Yes | | The DataTable to write. |
| `HasHeaders` | Has Headers | `InArgument` | `Boolean` | No | `True` | Whether to include column headers. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |
