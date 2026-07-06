# For Each List

`UiPath.MicrosoftOffice365.Activities.Sharepoint.ForEachListConnections`

Iterates over all lists in a SharePoint site.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `SiteUrlOrId` | Site URL or ID | `InArgument` | `String` | Yes | | The SharePoint site URL or ID to retrieve lists from. |
| `IncludeColumnsDefinitions` | Include Column Definitions | `InArgument` | `Boolean` | No | `False` | When true, includes column definitions for each list. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Length` | Length | `OutArgument` | `Int32` | The number of lists processed. |

### Body

Each iteration provides:
- `CurrentItem` ([`Office365SharepointList`](types/Office365SharepointList.md)) -- the current list.
- `CurrentListIndex` (`Int32`) -- the zero-based iteration index.
