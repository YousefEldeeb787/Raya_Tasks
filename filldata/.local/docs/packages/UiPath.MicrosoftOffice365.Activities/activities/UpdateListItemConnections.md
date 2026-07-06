# Update List Item

`UiPath.MicrosoftOffice365.Activities.Sharepoint.UpdateListItemConnections`

Updates an existing item in a SharePoint list.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `List` | List | `Property` | [`ListArgument`](components/ListArgument.md) | Yes | | The SharePoint list containing the item. See [ListArgument](components/ListArgument.md) for input modes. |
| `ItemId` | Item ID | `InArgument` | `String` | Yes | | The ID of the item to update. |
| `DataRow` | Data Row | `InArgument` | `DataRow` | No | | A DataRow containing the field values to update. Used when fields are specified dynamically. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | `DataRow` | The updated list item as a DataRow. |
