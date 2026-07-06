# Write Row

`UiPath.MicrosoftOffice365.Activities.Excel.WriteRowConnections`

Writes data to a row in an Excel Online workbook.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Workbook | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The workbook to write to. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `RangeLocation` | Range Location | `InArgument` | `String` | Yes | | The sheet name or named range. |
| `Range` | Range | `InArgument` | `String` | Yes | | The starting cell for the row (e.g., "A1"). |
| `WriteMode` | Write Mode | `InArgument` | `RangeWriteMode` | No | `Overwrite` | How to add data: `Overwrite`, `Append`. |
| `DataType` | Data Type | `Property` | `InputDataType` | No | | The type of input data: `DataRow` or `ArrayRow`. |
| `DataRow` | Data Row | `InArgument` | `DataRow` | When DataRow | | The DataRow to write. |
| `ArrayRow` | Array Row | `InArgument` | `Object[]` | When ArrayRow | | The array of values to write. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |
