# Get List Info

`UiPath.MicrosoftOffice365.Activities.Sharepoint.GetListInfo`

Retrieves information about a SharePoint list including its columns and metadata.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `List` | List | `Property` | [`ListArgument`](components/ListArgument.md) | Yes | | The SharePoint list to retrieve info for. See [ListArgument](components/ListArgument.md) for input modes. |
| `IncludeColumnDefinitions` | Include Column Definitions | `InArgument` | `Boolean` | No | `False` | When true, includes column definitions in the response. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | [`Office365SharepointList`](types/Office365SharepointList.md) | The SharePoint list information. |
