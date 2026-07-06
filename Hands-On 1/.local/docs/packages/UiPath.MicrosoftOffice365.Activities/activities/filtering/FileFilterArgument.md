# FileFilterArgument

**Namespace:** `UiPath.MicrosoftOffice365.Activities.Files.Filters`

Filters OneDrive/SharePoint files using the Microsoft Graph API OData filter syntax. Used by activities like GetFileListConnections and ForEachFileFolderConnections (when using the simple filter mode).

## Properties

| Property | Type | Description |
|----------|------|-------------|
| `LogicalOperator` | `FileFilterLogicalOperator` | Logical operator between conditions (`And`, `Or`). |
| `Filters` | `List<SingleFileFilterArgument>` | Zero or more filter conditions to apply. |

## SingleFileFilterArgument

Each filter element specifies a field, operator, and value.

| Property | Type | Description |
|----------|------|-------------|
| `Criteria` | `FileFilterField` | The field to filter on. |
| `StringOperator` | `FileStringFilterOperator` | Operator for string fields (e.g., `Equals`, `StartsWith`, `Contains`). |
| `FileExtensionStringOperator` | `FileExtensionStringFilterOperator` | Operator for file extension fields (`Equals`, `NotEqual`). |
| `DateOperator` | `FileDateFilterOperator` | Operator for date fields. |
| `IntOperator` | `FileIntFilterOperator` | Operator for integer fields (e.g., size). |
| `InStringValue` | `InArgument<String>` | String value for the filter. |
| `InFileExtensionStringValue` | `InArgument<String>` | String value for file extension filter. |
| `DateValue` | `InArgument<DateTime>` | Date value for date filters. |
| `IntValue` | `InArgument<Int32>` | Integer value for numeric filters. |

## FileFilterField Enum

| Value | Description |
|-------|-------------|
| `FileExtension` | Filter by file extension. |
| `FileName` | Filter by file name. |
| `CreatedBy` | Filter by creator. |
| `CreationDate` | Filter by creation date. |
| `LastModifiedBy` | Filter by last modifier. |
| `LastModifiedDate` | Filter by last modified date. |
| `SharedWithUser` | Filter by shared-with user. |
| `Size` | Filter by file size. |

## Cross-References

- Filters results of type [O365DriveRemoteItem](../types/O365DriveRemoteItem.md)
- Used alongside [DriveItemArgument](../components/DriveItemArgument.md) for file selection
- For search-based filtering (KQL), see [FileFolderFilterArgument](FileFolderFilterArgument.md)
