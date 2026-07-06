# Add List Item

`UiPath.MicrosoftOffice365.Activities.Sharepoint.AddListItemConnections`

Adds a single item to a SharePoint list.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `List` | List | `Property` | [`ListArgument`](components/ListArgument.md) | Yes | | The SharePoint list to add the item to. See [ListArgument](components/ListArgument.md) for input modes. |
| `DataRow` | Data Row | `InArgument` | `DataRow` | No | | A DataRow containing the field values. Used when fields are specified dynamically. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | `DataRow` | The newly added list item as a DataRow. |
