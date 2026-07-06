# Office365SharepointListItem

**Namespace:** `UiPath.MicrosoftOffice365.Models`

Represents an item (row) in a SharePoint list. Returned by activities such as GetListItemsConnections, GetSingleSharepointListItemConnections, ForEachListItemConnections, AddListItemConnections, and UpdateListItemConnections.

## Properties

| Property | Type | Description |
|----------|------|-------------|
| `Id` | `String` | The unique identifier of the list item. |
| `CreatedDateTime` | `DateTime` | When the item was created. |
| `LastModifiedDateTime` | `DateTime` | When the item was last modified. |
| `Fields` | `Office365ListItemField[]` | The field values for this list item. |

## Office365ListItemField

Represents a single field (column value) in a list item.

| Property | Type | Description |
|----------|------|-------------|
| `DisplayName` | `String` | The display name (title) of the field. Can differ from the internal name. |
| `Name` | `String` | The internal name of the field. This is the key used in the Graph API response. |
| `ReadOnly` | `Boolean` | `true` if the field is read-only. |
| `Value` | `Object` | The actual value of the field. |

## Cross-References

- Returned by SharePoint list item activities (GetListItemsConnections, ForEachListItemConnections, etc.)
- Part of [Office365SharepointList](Office365SharepointList.md)
- List selection uses [ListArgument](../components/ListArgument.md)
- Filtered by [ColumnFilterCollection](../filtering/ColumnFilterCollection.md) and [ColumnByNameFilterCollection](../filtering/ColumnByNameFilterCollection.md)
