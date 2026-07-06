# SharePoint List Item Added (Trigger)

`UiPath.MicrosoftOffice365.Activities.Sharepoint.Triggers.SharepointListItemAdded`

Trigger that fires when a new item is added to the monitored SharePoint list.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `List` | List | `Property` | [`ListArgument`](components/ListArgument.md) | Yes | | The SharePoint list to monitor. See [ListArgument](components/ListArgument.md) for input modes. |
| `Filter` | Filter | `Property` | [`ColumnFilterCollection`](filtering/ColumnFilterCollection.md) | No | | Filter criteria for the new item. See [ColumnFilterCollection](filtering/ColumnFilterCollection.md). |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | `DataRow` | The newly added list item. |
