# Get Single SharePoint List Item

`UiPath.MicrosoftOffice365.Activities.Sharepoint.GetSingleSharepointListItemConnections`

Retrieves a single item from a SharePoint list by its ID.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `List` | List | `Property` | [`ListArgument`](components/ListArgument.md) | Yes | | The SharePoint list to retrieve the item from. See [ListArgument](components/ListArgument.md) for input modes. |
| `ItemId` | Item ID | `InArgument` | `String` | Yes | | The ID of the item to retrieve. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | `DataRow` | The list item as a DataRow. |
