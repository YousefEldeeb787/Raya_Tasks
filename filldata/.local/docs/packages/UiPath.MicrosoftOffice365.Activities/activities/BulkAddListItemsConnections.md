# Bulk Add List Items

`UiPath.MicrosoftOffice365.Activities.Sharepoint.BulkAddListItemsConnections`

Adds multiple items to a SharePoint list from a DataTable.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `List` | List | `Property` | [`ListArgument`](components/ListArgument.md) | Yes | | The SharePoint list to add items to. See [ListArgument](components/ListArgument.md) for input modes. |
| `DataTable` | Data Table | `InArgument` | `DataTable` | Yes | | The DataTable containing the items to add. Column names must match list column names. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | `DataTable` | The added items with their SharePoint IDs. |
