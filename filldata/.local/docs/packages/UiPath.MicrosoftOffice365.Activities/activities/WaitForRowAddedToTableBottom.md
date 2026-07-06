# Wait For Row Added To Table Bottom

`UiPath.MicrosoftOffice365.Activities.WaitForRowAddedToTableBottom`

Pauses the workflow and waits until a row is added at the bottom of a table in the monitored Excel Online workbook.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Workbook | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The workbook to monitor. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `Range` | Table Name | `InArgument` | `String` | Yes | | The name of the table to monitor. |
| `Timeout` | Timeout | `InArgument` | `TimeSpan` | No | | Maximum time to wait before timing out. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | `DataRow` | The newly added row. |
