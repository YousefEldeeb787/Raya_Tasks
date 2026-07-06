# ListArgument

**Namespace:** `UiPath.MicrosoftOffice365.Activities.Sharepoint.Models`

A composition argument object used by SharePoint list activities to specify which list to operate on. The `ItemSelectionMode` property determines which sub-properties are required.

## InputMode -- ItemSelectionMode (Sharepoint.Enums.ItemSelectionMode)

| Mode | Description | Required Properties | AI-XAML Suitable |
|------|-------------|--------------------|--------------------|
| `Browse` | Select a SharePoint list via the Studio designer browser widget. | `BrowserListId`, `BrowserSiteUrl` | **Not suitable for AI-generated XAML** |
| `Manually` | Specify a SharePoint list by name/ID and site URL. | `ManualEntryListNameOrId`, `ManualEntrySiteUrl` | Yes |

## Properties

### Core

| Property | Type | Description |
|----------|------|-------------|
| `ItemSelectionMode` | `Sharepoint.Enums.ItemSelectionMode` | Determines whether the list is selected via the browser or entered manually. Default: `Browse`. |
| `ConnectionKey` | `String` | The connection ID captured when the SharePoint connection was selected. |
| `ConnectionDescriptor` | `String` | A human-readable label for the connection. |

### Manually Mode

| Property | Type | Description |
|----------|------|-------------|
| `ManualEntryListNameOrId` | `InArgument<String>` | The list name or ID. **Required.** |
| `ManualEntrySiteUrl` | `InArgument<String>` | The SharePoint site URL. **Required.** |

### Browse Mode (designer-only)

| Property | Type | Description |
|----------|------|-------------|
| `BrowserListId` | `InArgument<String>` | The SharePoint list ID from the browser widget. `[AutopilotIgnored]` |
| `BrowserListFriendlyName` | `InArgument<String>` | The display name of the selected list. |
| `BrowserSiteUrl` | `InArgument<String>` | The SharePoint site URL from the browser widget. |

### Other

| Property | Type | Description |
|----------|------|-------------|
| `Backup` | `BackupSlot<ItemSelectionMode>` | Preserves the previous mode when switching. |

## XAML Examples

### Manually Mode
```xml
<spmodels:ListArgument ItemSelectionMode="Manually">
    <spmodels:ListArgument.ManualEntryListNameOrId>
        <InArgument x:TypeArguments="x:String">["My Tasks"]</InArgument>
    </spmodels:ListArgument.ManualEntryListNameOrId>
    <spmodels:ListArgument.ManualEntrySiteUrl>
        <InArgument x:TypeArguments="x:String">["https://contoso.sharepoint.com/sites/mysite"]</InArgument>
    </spmodels:ListArgument.ManualEntrySiteUrl>
</spmodels:ListArgument>
```

## Variant: TriggerListArgument

A simplified version used by SharePoint list triggers. Properties are stored as plain strings instead of `InArgument<T>`.

| Property | Type | Description |
|----------|------|-------------|
| `BrowserListId` | `String` | The SharePoint list ID. `[AutopilotIgnored]` |
| `BrowserListName` | `String` | The display name of the selected list. |
| `BrowserSiteUrl` | `String` | The SharePoint site URL. |
| `ManualEntryListName` | `String` | The list name entered manually. |
| `ManualEntrySiteUrl` | `String` | The site URL entered manually. |

## Cross-References

- Selects lists of type [Office365SharepointList](../types/Office365SharepointList.md)
- Used by SharePoint list activities (GetListItemsConnections, ForEachListItemConnections, AddListItemConnections, etc.)
- List items are of type [Office365SharepointListItem](../types/Office365SharepointListItem.md)
- Items can be filtered by [ColumnFilterCollection](../filtering/ColumnFilterCollection.md) and [ColumnByNameFilterCollection](../filtering/ColumnByNameFilterCollection.md)
