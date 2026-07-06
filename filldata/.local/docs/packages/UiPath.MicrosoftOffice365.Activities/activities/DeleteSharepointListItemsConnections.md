# Delete SharePoint List Items

`UiPath.MicrosoftOffice365.Activities.Sharepoint.DeleteSharepointListItemsConnections`

Deletes items from a SharePoint list by their IDs.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `List` | List | `Property` | [`ListArgument`](components/ListArgument.md) | Yes | | The SharePoint list to delete items from. See [ListArgument](components/ListArgument.md) for input modes. |
| `ItemIds` | Item IDs | `InArgument` | `IEnumerable<String>` | Yes | | The IDs of the items to delete. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | `DataTable` | The deleted items. |
