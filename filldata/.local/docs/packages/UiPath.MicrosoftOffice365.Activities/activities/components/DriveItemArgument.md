# DriveItemArgument

**Namespace:** `UiPath.MicrosoftOffice365.Activities.Files.Models`

A composition argument object used by OneDrive and SharePoint file activities to specify a file or folder. The `ItemSelectionMode` property determines which sub-properties are required.

## InputMode — ItemSelectionMode (EDriveItemMode)

| Mode | Description | Required Properties | AI-XAML Suitable |
|------|-------------|--------------------|--------------------|
| `Browse` | Select a file/folder via the Studio designer browser widget. | `BrowserItemId`, `BrowserDriveId`, `BrowserDriveName`, `BrowserSiteUrl` | **Not suitable for AI-generated XAML** |
| `ItemId` | Specify a file/folder by its OneDrive/SharePoint item ID. | `ManualEntryItemId` (required); `ManualEntryDriveName`, `ManualEntrySiteUrl` (optional, for SharePoint) | Yes |
| `UseExisting` | Pass an existing [O365DriveRemoteItem](../types/O365DriveRemoteItem.md) reference. | `Item` | Yes |
| `ItemUrl` | Specify a file/folder by its web URL. | `ManualEntryItemUrl` | Yes |
| `FullPath` | Specify a file/folder by its full path from the drive root. | `ManualEntryItemFullPath` | Yes |
| `RelativePath` | Specify a path relative to a browsed parent folder. | `BrowserParentItemId`, `BrowserParentDriveId`, `ManualEntryItemRelativePath` | No (requires browsed parent) |

## Properties

### Core

| Property | Type | Description |
|----------|------|-------------|
| `ItemSelectionMode` | `EDriveItemMode` | Determines how the drive item is specified. |
| `ConnectionKey` | `String` | The connection key identifying which integration service connection was used at design time. |
| `ConnectionDescriptor` | `String` | A human-readable descriptor for the connection. |

### ItemId Mode

| Property | Type | Description |
|----------|------|-------------|
| `ManualEntryItemId` | `InArgument<String>` | The item identifier. **Required for ItemId mode.** Note: marked `[AutopilotIgnored]` but still the required property for this mode -- use it. |
| `ManualEntryDriveName` | `InArgument<String>` | The document library name (for SharePoint). If set, `ManualEntrySiteUrl` is also required. |
| `ManualEntrySiteUrl` | `InArgument<String>` | The SharePoint site URL (required when `ManualEntryDriveName` is set). |

### ItemUrl Mode

| Property | Type | Description |
|----------|------|-------------|
| `ManualEntryItemUrl` | `InArgument<String>` | The web URL that identifies the file or folder. |

### UseExisting Mode

| Property | Type | Description |
|----------|------|-------------|
| `Item` | `InArgument<O365DriveRemoteItem>` | An existing drive item reference from a previous activity output. |

### FullPath Mode

| Property | Type | Description |
|----------|------|-------------|
| `ManualEntryItemFullPath` | `InArgument<String>` | The full path from the drive root (e.g., `/Documents/Reports/Q1.xlsx`). |

### RelativePath Mode

| Property | Type | Description |
|----------|------|-------------|
| `BrowserParentItemId` | `InArgument<String>` | The ID of the parent folder (browsed). `[AutopilotIgnored]` |
| `BrowserParentDriveId` | `InArgument<String>` | The drive ID of the parent folder (browsed). `[AutopilotIgnored]` |
| `ManualEntryItemRelativePath` | `InArgument<String>` | The path relative to the parent folder. |

### Browse Mode (designer-only)

| Property | Type | Description |
|----------|------|-------------|
| `BrowserItemId` | `InArgument<String>` | Item ID from browser. `[AutopilotIgnored]` |
| `BrowserDriveId` | `InArgument<String>` | Drive ID from browser. `[AutopilotIgnored]` |
| `BrowserDriveName` | `InArgument<String>` | Drive name from browser. |
| `BrowserSiteUrl` | `InArgument<String>` | Site URL from browser. |
| `BrowserSpecificUrl` | `InArgument<String>` | Specific URL from browser. |
| `BrowserItemFriendlyName` | `InArgument<String>` | Friendly display name from browser. |
| `ReferenceId` | `String` | Internal reference ID. `[AutopilotIgnored]` |
| `FullPathHint` | `String` | Full path hint for fallback resolution. |

### Other

| Property | Type | Description |
|----------|------|-------------|
| `ItemIdBackup` | `BackupSlot<EDriveItemMode>` | Stores previous mode for undo. `[AutopilotIgnored]` |

## XAML Examples

### ItemUrl Mode
```xml
<models:DriveItemArgument ItemSelectionMode="ItemUrl">
    <models:DriveItemArgument.ManualEntryItemUrl>
        <InArgument x:TypeArguments="x:String">[fileUrl]</InArgument>
    </models:DriveItemArgument.ManualEntryItemUrl>
</models:DriveItemArgument>
```

### UseExisting Mode
```xml
<models:DriveItemArgument ItemSelectionMode="UseExisting">
    <models:DriveItemArgument.Item>
        <InArgument x:TypeArguments="files:O365DriveRemoteItem">[existingItem]</InArgument>
    </models:DriveItemArgument.Item>
</models:DriveItemArgument>
```

### FullPath Mode
```xml
<models:DriveItemArgument ItemSelectionMode="FullPath">
    <models:DriveItemArgument.ManualEntryItemFullPath>
        <InArgument x:TypeArguments="x:String">["/Documents/Report.xlsx"]</InArgument>
    </models:DriveItemArgument.ManualEntryItemFullPath>
</models:DriveItemArgument>
```

### ItemId Mode
```xml
<models:DriveItemArgument ItemSelectionMode="ItemId">
    <models:DriveItemArgument.ManualEntryItemId>
        <InArgument x:TypeArguments="x:String">[itemId]</InArgument>
    </models:DriveItemArgument.ManualEntryItemId>
    <models:DriveItemArgument.ManualEntrySiteUrl>
        <InArgument x:TypeArguments="x:String">["https://contoso.sharepoint.com/sites/mysite"]</InArgument>
    </models:DriveItemArgument.ManualEntrySiteUrl>
    <models:DriveItemArgument.ManualEntryDriveName>
        <InArgument x:TypeArguments="x:String">["Documents"]</InArgument>
    </models:DriveItemArgument.ManualEntryDriveName>
</models:DriveItemArgument>
```

## Cross-References

- Selects files/folders of type [O365DriveRemoteItem](../types/O365DriveRemoteItem.md)
- Used by OneDrive/SharePoint file activities (CopyItemConnections, MoveItemConnections, DownloadFileConnections, etc.)
- Files can be filtered by [FileFilterArgument](../filtering/FileFilterArgument.md) and [FileFolderFilterArgument](../filtering/FileFolderFilterArgument.md)
