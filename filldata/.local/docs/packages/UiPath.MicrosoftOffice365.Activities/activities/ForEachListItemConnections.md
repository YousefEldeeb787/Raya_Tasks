# For Each List Item

`UiPath.MicrosoftOffice365.Activities.Sharepoint.ForEachListItemConnections`

Iterates over items in a SharePoint list.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `List` | List | `Property` | [`ListArgument`](components/ListArgument.md) | Yes | | The SharePoint list to iterate over. See [ListArgument](components/ListArgument.md) for input modes. |
| `Filter` | Filter | `Property` | [`ColumnFilterCollection`](filtering/ColumnFilterCollection.md) | No | | Filter criteria for list items. See [ColumnFilterCollection](filtering/ColumnFilterCollection.md). |
| `MaxResults` | Max Results | `InArgument` | `Int32` | No | | Maximum number of items to return. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Length` | Length | `OutArgument` | `Int32` | The number of items processed. |

### Body

Each iteration provides:
- `CurrentItem` (`DataRow`) -- the current list item.
- `CurrentItemIndex` (`Int32`) -- the zero-based iteration index.
