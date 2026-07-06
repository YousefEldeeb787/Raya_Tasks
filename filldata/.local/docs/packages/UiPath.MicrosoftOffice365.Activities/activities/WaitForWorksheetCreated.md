# Wait For Worksheet Created

`UiPath.MicrosoftOffice365.Activities.WaitForWorksheetCreated`

Pauses the workflow and waits until a new worksheet is created in the monitored Excel Online workbook.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Workbook | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The workbook to monitor. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `Timeout` | Timeout | `InArgument` | `TimeSpan` | No | | Maximum time to wait before timing out. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | `String` | The name of the newly created worksheet. |
