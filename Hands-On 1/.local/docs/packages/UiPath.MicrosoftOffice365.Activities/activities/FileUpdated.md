# File Updated (Trigger)

`UiPath.MicrosoftOffice365.Activities.Files.Triggers.FileUpdated`

Trigger that fires when a file is updated in the monitored OneDrive or SharePoint folder.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Folder | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The folder to monitor for file updates. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `Filter` | Filter | `Property` | [`FileFolderFilterArgument`](filtering/FileFolderFilterArgument.md) | No | | Filter criteria for the updated file. See [FileFolderFilterArgument](filtering/FileFolderFilterArgument.md). |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | [`O365DriveRemoteItem`](types/O365DriveRemoteItem.md) | The updated file. |
