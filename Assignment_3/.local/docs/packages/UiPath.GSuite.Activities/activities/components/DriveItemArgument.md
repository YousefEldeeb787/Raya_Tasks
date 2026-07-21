# DriveItemArgument

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Activities.Models.DriveItemArgument`

A composition component used by Google Drive activities to specify a target file or folder. Supports multiple input modes so users can identify items by browsing, URL/ID, existing variable, full path, or relative path.

**Assembly:** `UiPath.GSuite.Activities`
**Inherits:** `BaseDriveItemArgument`

## InputMode (`EDriveItemMode`)

Determines which properties are active.

| Mode | Value | Description | AI-XAML Suitable |
|------|-------|-------------|------------------|
| `Browse` | `0` | Select a file/folder from the remote browser in Studio. | **Not suitable for AI-generated XAML** -- requires interactive Studio UI. |
| `UrlOrId` | `1` | Manually enter a file/folder ID or URL. | Yes |
| `UseExisting` | `2` | Use an existing [`GDriveRemoteItem`](../types/GDriveRemoteItem.md) variable. | Yes |
| `FullPath` | `3` | Enter the absolute path from the drive root. | Yes |
| `RelativePath` | `4` | Specify a parent folder and a relative path from it. | Yes |

## Properties

| Property | Type | Mode(s) | Description |
|----------|------|---------|-------------|
| `InputMode` | `EDriveItemMode` | All | Determines how the file/folder is specified. |
| `Existing` | `InArgument<`[`GDriveRemoteItem`](../types/GDriveRemoteItem.md)`>` | UseExisting | An existing file/folder variable. |
| `IdOrUrl` | `InArgument<string>` | UrlOrId | File ID or file URL manually entered. |
| `BrowserId` | `InArgument<string>` | Browse | File ID saved when browsing. |
| `DriveId` | `InArgument<string>` | Browse | The drive ID of the file. Present if the file is in a shared drive. |
| `FriendlyName` | `InArgument<string>` | Browse | File friendly name from browsing. |
| `FullPath` | `InArgument<string>` | FullPath | The absolute path of the file starting from the root of the drive. |
| `FullPathHint` | `InArgument<string>` | Browse | The absolute path hint as specified by the browser (used for fallback resolution). |
| `ParentId` | `InArgument<string>` | RelativePath | The ID of the parent folder. |
| `ParentIdFriendlyName` | `InArgument<string>` | RelativePath | Friendly name of the parent folder. |
| `ParentIdFullPathHint` | `InArgument<string>` | RelativePath | The absolute path hint for the parent folder (used for fallback resolution). |
| `RelativePath` | `InArgument<string>` | RelativePath | Relative path with respect to the parent folder. |
| `ConnectionKey` | `string` | All | The ID of the connection from the moment the connection data was chosen. |
| `ConnectionDescriptor` | `string` | All | A user-friendly string describing the connection. |
| `Backup` | `BackupSlot<EDriveItemMode>` | All | Stores the previous InputMode value for designer revert. Studio **always** serializes this — every XAML example must include the `.Backup` child element. |

## XAML Examples

> **Supported `InputMode` values:** `UrlOrId` *(recommended for AI XAML)*, `UseExisting`, `FullPath`, `RelativePath`. `Browse` is available in Studio but requires interactive selection — not suitable for AI-generated XAML. `RelativePath` requires a parent folder browsed in Studio — also not suitable.

### UrlOrId Mode (recommended)

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:models="clr-namespace:UiPath.GSuite.Activities.Models;assembly=UiPath.GSuite.Activities"
    xmlns:driveEnums="clr-namespace:UiPath.GSuite.Activities.Drive.Enums;assembly=UiPath.GSuite.Activities"
    xmlns:usau="clr-namespace:UiPath.Shared.Activities.Utils;assembly=UiPath.GSuite.Activities"
    xmlns:scg="clr-namespace:System.Collections.Generic;assembly=mscorlib"
-->
<models:DriveItemArgument InputMode="UrlOrId">
  <models:DriveItemArgument.IdOrUrl>
    <InArgument x:TypeArguments="x:String">https://docs.google.com/document/d/abc123/edit</InArgument>
  </models:DriveItemArgument.IdOrUrl>
  <models:DriveItemArgument.Backup>
    <usau:BackupSlot x:TypeArguments="driveEnums:EDriveItemMode" StoredValue="UrlOrId">
      <usau:BackupSlot.BackupValues>
        <scg:Dictionary x:TypeArguments="driveEnums:EDriveItemMode, scg:List(x:Object)" />
      </usau:BackupSlot.BackupValues>
    </usau:BackupSlot>
  </models:DriveItemArgument.Backup>
</models:DriveItemArgument>
```

### UseExisting Mode

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:models="clr-namespace:UiPath.GSuite.Activities.Models;assembly=UiPath.GSuite.Activities"
    xmlns:drive="clr-namespace:UiPath.GSuite.Drive.Models;assembly=UiPath.GSuite"
    xmlns:driveEnums="clr-namespace:UiPath.GSuite.Activities.Drive.Enums;assembly=UiPath.GSuite.Activities"
    xmlns:usau="clr-namespace:UiPath.Shared.Activities.Utils;assembly=UiPath.GSuite.Activities"
    xmlns:scg="clr-namespace:System.Collections.Generic;assembly=mscorlib"
-->
<models:DriveItemArgument InputMode="UseExisting">
  <models:DriveItemArgument.Existing>
    <InArgument x:TypeArguments="drive:GDriveRemoteItem">[MyFileVariable]</InArgument>
  </models:DriveItemArgument.Existing>
  <models:DriveItemArgument.Backup>
    <usau:BackupSlot x:TypeArguments="driveEnums:EDriveItemMode" StoredValue="UseExisting">
      <usau:BackupSlot.BackupValues>
        <scg:Dictionary x:TypeArguments="driveEnums:EDriveItemMode, scg:List(x:Object)" />
      </usau:BackupSlot.BackupValues>
    </usau:BackupSlot>
  </models:DriveItemArgument.Backup>
</models:DriveItemArgument>
```

### FullPath Mode

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:models="clr-namespace:UiPath.GSuite.Activities.Models;assembly=UiPath.GSuite.Activities"
    xmlns:driveEnums="clr-namespace:UiPath.GSuite.Activities.Drive.Enums;assembly=UiPath.GSuite.Activities"
    xmlns:usau="clr-namespace:UiPath.Shared.Activities.Utils;assembly=UiPath.GSuite.Activities"
    xmlns:scg="clr-namespace:System.Collections.Generic;assembly=mscorlib"
-->
<models:DriveItemArgument InputMode="FullPath">
  <models:DriveItemArgument.FullPath>
    <InArgument x:TypeArguments="x:String">My Drive/Documents/report.pdf</InArgument>
  </models:DriveItemArgument.FullPath>
  <models:DriveItemArgument.Backup>
    <usau:BackupSlot x:TypeArguments="driveEnums:EDriveItemMode" StoredValue="FullPath">
      <usau:BackupSlot.BackupValues>
        <scg:Dictionary x:TypeArguments="driveEnums:EDriveItemMode, scg:List(x:Object)" />
      </usau:BackupSlot.BackupValues>
    </usau:BackupSlot>
  </models:DriveItemArgument.Backup>
</models:DriveItemArgument>
```

## Notes

- When the connection changes between design time and runtime, Browse and RelativePath modes attempt to re-resolve items by path hint as a fallback.
- The `Undefined` enum value (value -999) is obsolete and exists only for Studio Web compatibility.
- `BrowserId`, `DriveId`, and `ParentId` have the `[AutopilotIgnored]` attribute -- they are regular properties but are typically populated by the Studio browser, not by users directly.
- `RelativePath` mode requires a parent folder that was previously browsed in Studio (`ParentId` populated). Not suitable for AI-generated XAML.
- Every XAML example that contains a `DriveItemArgument` must include a `.Backup` child element with `StoredValue` matching the active `InputMode` and an empty `BackupValues` dictionary.

## Cross-References

- **Output type:** [`GDriveRemoteItem`](../types/GDriveRemoteItem.md) — returned by Drive activities that output a file/folder reference
- **Filtering:** [`FileFilterArgument`](../filtering/FileFilterArgument.md) — used alongside `DriveItemArgument` in list/search activities
- **Activities that use this component:** CopyFileConnections, MoveFileConnections, GetFileFolderConnections, GetFileFolderInfoConnections, DownloadFileConnections, UploadFilesConnections, DeleteFileOrFolderConnections, ShareFileFolderConnections, RenameFileFolderConnections, CreateFolderConnections, FileFolderExistsConnections, ForEachFileFolderConnections, GetFileListConnections, and all Sheets/Docs activities that target a spreadsheet or document file
