# Get List Items

`UiPath.MicrosoftOffice365.Activities.Sharepoint.GetListItemsConnections`

Retrieves items from a SharePoint list and returns them as a DataTable.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `List` | List | `Property` | [`ListArgument`](components/ListArgument.md) | Yes | | The SharePoint list to retrieve items from. See [ListArgument](components/ListArgument.md) for input modes. |
| `Filter` | Filter | `Property` | [`ColumnFilterCollection`](filtering/ColumnFilterCollection.md) | No | | Filter criteria for list items. See [ColumnFilterCollection](filtering/ColumnFilterCollection.md). |
| `ItemIds` | Item IDs | `InArgument` | `IEnumerable<String>` | No | | Specific item IDs to retrieve. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | `DataTable` | The list items as a DataTable. |
