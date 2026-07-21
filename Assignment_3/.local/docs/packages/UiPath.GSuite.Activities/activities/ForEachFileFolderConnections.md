# For Each File/Folder

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.ForEachFileFolderConnections`

Iterates over files and folders in a Google Drive location, executing the body for each item. This is a `NativeActivity` that supports an `ActivityAction` body.

**Package:** `UiPath.GSuite.Activities`
**Category:** Drive
**Connector:** `uipath-google-drive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Folder | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | No | | The Google Drive folder to iterate through. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `CurrentItemVariableName` | Current Item Variable Name | `Property` | `string` | Yes | `CurrentItem` | The variable name for the current [`GDriveRemoteItem`](types/GDriveRemoteItem.md) in each iteration. |
| `MaxResults` | Max Results | `InArgument` | `int` | No | `200` | The maximum number of files and folders to return. |
| `WhatToReturn` | What to Return | `InArgument` | `LocationType` | No | `FilesAndFolders` | Whether to return only files, only folders, or both. |
| `StarredOnly` | Starred Only | `InArgument` | `bool` | No | `false` | Whether to return only starred files and folders. |
| `FilterSelectionMode` | Filter Selection Mode | `Property` | `FilterMode` | No | `ConditionBuilder` | Whether to use the condition builder or a raw query string. |
| `QueryFilter` | Query Filter | `InArgument` | `string` | No | | Raw Google Drive query string. Used when `FilterSelectionMode` is `Query`. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google Workspace connection to use. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `Filter` | Filter | [`FileFilterArgument`](filtering/FileFilterArgument.md) | | See [FileFilterArgument](filtering/FileFilterArgument.md) for criteria and operators. |
| `Body` | Body | `ActivityAction<GDriveRemoteItem, int>` | | The activity body executed for each [`GDriveRemoteItem`](types/GDriveRemoteItem.md). Receives the current item and its zero-based index. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Length` | Length | `OutArgument` | `int` | The number of files/folders processed. |

## Enum Reference

### `LocationType`
| Value | Description |
|-------|-------------|
| `FilesAndFolders` | Return both files and folders |
| `Files` | Return only files |
| `Folders` | Return only folders |

### `FilterMode`
| Value | Description |
|-------|-------------|
| `ConditionBuilder` | Filter using the condition builder |
| `Query` | Filter using a raw Google Drive query string |

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
<gsuite:ForEachFileFolderConnections
    DisplayName="For Each File/Folder"
    ConnectionId="{x:Null}"
    CurrentItemVariableName="currentItem"
    WhatToReturn="[LocationType.Files]"
    MaxResults="[100]"
    StarredOnly="[False]"
    FilterSelectionMode="Query"
    QueryFilter="[&quot;name contains 'report'&quot;]"
    Length="[itemCount]">
    <gsuite:ForEachFileFolderConnections.Item>
        <models:DriveItemArgument InputMode="UrlOrId">
            <models:DriveItemArgument.IdOrUrl>
                <InArgument x:TypeArguments="x:String">[folderId]</InArgument>
            </models:DriveItemArgument.IdOrUrl>
          <models:DriveItemArgument.Backup>
    <usau:BackupSlot x:TypeArguments="driveEnums:EDriveItemMode" StoredValue="UrlOrId">
      <usau:BackupSlot.BackupValues>
        <scg:Dictionary x:TypeArguments="driveEnums:EDriveItemMode, scg:List(x:Object)" />
      </usau:BackupSlot.BackupValues>
    </usau:BackupSlot>
  </models:DriveItemArgument.Backup>
</models:DriveItemArgument>
    </gsuite:ForEachFileFolderConnections.Item>
    <gsuite:ForEachFileFolderConnections.Body>
        <ActivityAction x:TypeArguments="drive:GDriveRemoteItem, x:Int32">
            <ActivityAction.Argument1>
                <DelegateInArgument x:TypeArguments="drive:GDriveRemoteItem" Name="currentItem" />
            </ActivityAction.Argument1>
            <ActivityAction.Argument2>
                <DelegateInArgument x:TypeArguments="x:Int32" Name="CurrentItemIndex" />
            </ActivityAction.Argument2>
            <Sequence DisplayName="Do">
                <!-- body activities here -->
            </Sequence>
        </ActivityAction>
    </gsuite:ForEachFileFolderConnections.Body>
</gsuite:ForEachFileFolderConnections>
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- Requires a Google Workspace connection with Drive and Drive Labels scopes.
- Results are sorted by name.
- The body receives two arguments: the current [`GDriveRemoteItem`](types/GDriveRemoteItem.md) and the zero-based iteration index.
