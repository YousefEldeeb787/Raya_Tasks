# Get File List

`UiPath.MicrosoftOffice365.Activities.Files.GetFileListConnections`

Returns a collection of files and/or folders from OneDrive or SharePoint.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Folder | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The folder to list files from. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `Filter` | Filter | `Property` | [`FileFilterArgument`](filtering/FileFilterArgument.md) | No | | Filter criteria for files and folders. See [FileFilterArgument](filtering/FileFilterArgument.md). |
| `MaxResults` | Max Results | `InArgument` | `Int32` | No | `200` | Maximum number of items to return. |
| `WhatToReturn` | What to Return | `InArgument` | `FindFilesAndFoldersResultType` | No | `Files` | Whether to return files, folders, or both: `Files`, `Folders`, `FilesAndFolders`. |
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
| `Result` | Result | `OutArgument` | [`O365DriveRemoteItem`](types/O365DriveRemoteItem.md)`[]` | The collection of matching files and/or folders. |

## XAML Example

```xml
<o365files:GetFileListConnections DisplayName="Get File List" ConnectionId="[conn]"
    Result="[fileList]">
    <o365files:GetFileListConnections.Item>
        <models:DriveItemArgument ItemSelectionMode="ItemUrl">
            <models:DriveItemArgument.ManualEntryItemUrl>
                <InArgument x:TypeArguments="x:String">[folderUrl]</InArgument>
            </models:DriveItemArgument.ManualEntryItemUrl>
        </models:DriveItemArgument>
    </o365files:GetFileListConnections.Item>
</o365files:GetFileListConnections>
```
