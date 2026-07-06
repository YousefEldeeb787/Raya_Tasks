# Delete Range

`UiPath.MicrosoftOffice365.Activities.Excel.DeleteRangeConnections`

Deletes a range from an Excel Online workbook.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Workbook | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The workbook containing the range. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `Range` | Range | `InArgument` | `String` | Yes | | The range to delete (e.g., "Sheet1!A1:D10"). |
| `DeleteMode` | Delete Mode | `InArgument` | `RangeDeleteMode` | No | `None` | How to handle the deletion: `None`, `ShiftUp`, `ShiftLeft`. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |
