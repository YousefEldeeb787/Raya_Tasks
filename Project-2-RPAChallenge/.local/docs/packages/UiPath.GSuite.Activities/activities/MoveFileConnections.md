# Move File

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.MoveFileConnections`

Moves a file to a different folder in Google Drive, optionally renaming it.

**Package:** `UiPath.GSuite.Activities`
**Category:** Drive
**Connector:** `uipath-google-drive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | File | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The source file to move. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `Destination` | Destination Folder | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The destination folder. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `NewFileName` | New File Name | `InArgument` | `string` | No | | New name of the moved file. If empty, keeps the original name. |
| `ConflictResolution` | Conflict Resolution | `InArgument` | `ConflictBehavior` | No | `AddSeparate` | Behavior when a file with the same name already exists. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google Workspace connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | [`GDriveRemoteItem`](types/GDriveRemoteItem.md) | Reference to the moved file in its new location. |

## Output Model

Returns a [`GDriveRemoteItem`](types/GDriveRemoteItem.md) with file/folder ID, name, URL, MIME type, dates, and size.

## Enum Reference

### `ConflictBehavior`
| Value | Description |
|-------|-------------|
| `Replace` | Replace the existing item |
| `Fail` | Fail the request if another item with the same name exists |
| `Rename` | Rename the new item to have a unique name |
| `AddSeparate` | Add without renaming, even if same name exists |
| `UseExisting` | Return the existing item |

> **Supported `InputMode` values:** `UrlOrId` *(recommended for AI XAML)*, `UseExisting`, `FullPath`, `RelativePath`. `Browse` is available in Studio but requires interactive selection — not suitable for AI-generated XAML. `RelativePath` requires a parent folder browsed in Studio — also not suitable.

## XAML Example

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:gsuite="clr-namespace:UiPath.GSuite.Activities;assembly=UiPath.GSuite.Activities"
    xmlns:models="clr-namespace:UiPath.GSuite.Activities.Models;assembly=UiPath.GSuite.Activities"
    xmlns:driveEnums="clr-namespace:UiPath.GSuite.Activities.Drive.Enums;assembly=UiPath.GSuite.Activities"
    xmlns:usau="clr-namespace:UiPath.Shared.Activities.Utils;assembly=UiPath.GSuite.Activities"
    xmlns:scg="clr-namespace:System.Collections.Generic;assembly=mscorlib"
-->
<gsuite:MoveFileConnections
    DisplayName="Move File"
    ConnectionId="{x:Null}"
    NewFileName="[newName]"
    ConflictResolution="[ConflictBehavior.AddSeparate]"
    Result="[movedFile]">
    <gsuite:MoveFileConnections.Item>
        <models:DriveItemArgument InputMode="UrlOrId">
            <models:DriveItemArgument.IdOrUrl>
                <InArgument x:TypeArguments="x:String">[fileIdOrUrl]</InArgument>
            </models:DriveItemArgument.IdOrUrl>
          <models:DriveItemArgument.Backup>
    <usau:BackupSlot x:TypeArguments="driveEnums:EDriveItemMode" StoredValue="UrlOrId">
      <usau:BackupSlot.BackupValues>
        <scg:Dictionary x:TypeArguments="driveEnums:EDriveItemMode, scg:List(x:Object)" />
      </usau:BackupSlot.BackupValues>
    </usau:BackupSlot>
  </models:DriveItemArgument.Backup>
</models:DriveItemArgument>
    </gsuite:MoveFileConnections.Item>
    <gsuite:MoveFileConnections.Destination>
        <models:DriveItemArgument InputMode="UrlOrId">
            <models:DriveItemArgument.IdOrUrl>
                <InArgument x:TypeArguments="x:String">[destinationFolderId]</InArgument>
            </models:DriveItemArgument.IdOrUrl>
          <models:DriveItemArgument.Backup>
    <usau:BackupSlot x:TypeArguments="driveEnums:EDriveItemMode" StoredValue="UrlOrId">
      <usau:BackupSlot.BackupValues>
        <scg:Dictionary x:TypeArguments="driveEnums:EDriveItemMode, scg:List(x:Object)" />
      </usau:BackupSlot.BackupValues>
    </usau:BackupSlot>
  </models:DriveItemArgument.Backup>
</models:DriveItemArgument>
    </gsuite:MoveFileConnections.Destination>
</gsuite:MoveFileConnections>
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- Requires a Google Workspace connection with Drive scope.
