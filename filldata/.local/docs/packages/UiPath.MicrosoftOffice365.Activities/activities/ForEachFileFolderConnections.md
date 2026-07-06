# For Each File/Folder

`UiPath.MicrosoftOffice365.Activities.Files.ForEachFileFolderConnections`

Iterates over files and/or folders in a specified OneDrive or SharePoint location.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Folder | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The folder to iterate over. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `Filter` | Filter | `Property` | [`FileFilterArgument`](filtering/FileFilterArgument.md) | No | | Filter criteria for files and folders. See [FileFilterArgument](filtering/FileFilterArgument.md). |
| `MaxResults` | Max Results | `InArgument` | `Int32` | No | `200` | Maximum number of items to return. |
| `WhatToReturn` | What to Return | `InArgument` | `FindFilesAndFoldersResultType` | No | `FilesAndFolders` | Whether to return files, folders, or both: `Files`, `Folders`, `FilesAndFolders`. |
| `IncludeSubfolders` | Include Subfolders | `InArgument` | `Boolean` | No | `False` | When true, includes items from subfolders. |
| `TrimDuplicates` | Trim Duplicates | `InArgument` | `Boolean` | No | `False` | When true, removes duplicate results. |
| `SimpleSearch` | Search | `InArgument` | `String` | No | | A keyword-based search query. |
| `SortOptions` | Sort Options | `Property` | `SortOrder` | No | `Ascending` | Sort order by name: `Ascending`, `Descending`, `None`. |

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
- `CurrentItem` (`O365DriveRemoteItem`) -- the current file or folder.
- `CurrentItemIndex` (`Int32`) -- the zero-based iteration index.

## XAML Example

```xml
<o365files:ForEachFileFolderConnections DisplayName="For Each File" ConnectionId="[conn]">
    <o365files:ForEachFileFolderConnections.Item>
        <models:DriveItemArgument ItemSelectionMode="ItemUrl">
            <models:DriveItemArgument.ManualEntryItemUrl>
                <InArgument x:TypeArguments="x:String">[folderUrl]</InArgument>
            </models:DriveItemArgument.ManualEntryItemUrl>
        </models:DriveItemArgument>
    </o365files:ForEachFileFolderConnections.Item>
    <ActivityAction x:TypeArguments="files:O365DriveRemoteItem, x:Int32">
        <ActivityAction.Argument1><DelegateInArgument x:TypeArguments="files:O365DriveRemoteItem" Name="CurrentItem" /></ActivityAction.Argument1>
        <ActivityAction.Argument2><DelegateInArgument x:TypeArguments="x:Int32" Name="CurrentItemIndex" /></ActivityAction.Argument2>
        <Sequence DisplayName="Do">
            <!-- Activities here -->
        </Sequence>
    </ActivityAction>
</o365files:ForEachFileFolderConnections>
```
