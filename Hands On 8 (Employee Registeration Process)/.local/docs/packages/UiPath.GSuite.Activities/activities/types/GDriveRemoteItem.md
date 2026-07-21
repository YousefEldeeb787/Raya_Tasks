# GDriveRemoteItem

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Drive.Models.GDriveRemoteItem`

Represents a remote Google Drive file or folder. Exposes GDrive-specific properties and metadata beyond what the generic `IRemoteResource` interface provides. Activities within the GSuite package can leverage this type for direct cloud operations (e.g., copy/move without downloading locally).

**Assembly:** `UiPath.GSuite`
**Implements:** `IRemoteResource`
**Inherits:** `TraitProviderBase`

## Properties

| Property | Type | Description |
|----------|------|-------------|
| `Name` | `string` | Name of the resource. |
| `FullName` | `string` | Full name of the resource (the file name as stored in Drive). |
| `Extension` | `string` | File extension. |
| `CreationDate` | `DateTime?` | Creation date and time. |
| `LastModifiedDate` | `DateTime?` | Last modified date and time. |
| `ID` | `string` | The unique identifier of the file or folder. |
| `Url` | `string` | Web URL of the resource. |
| `Uri` | `string` | Web URL of the resource (legacy; prefer `Url`). |
| `IsFolder` | `bool` | True if the resource is a folder. |
| `MimeType` | `string` | The MIME type of the file. |
| `SizeInBytes` | `long` | Size of the file in bytes. |
| `IconUri` | `string` | URI to the file's icon. |
| `LocalCopy` | `ILocalResource` | A local copy handle that can download the file on demand. |
| `Metadata` | `Dictionary<string, string>` | Additional metadata key-value pairs. |
| `ParentId` | `string` | The URL or ID of the parent folder. If the item has multiple parents, the first one is returned. |
| `CreatedBy` | `GSuiteUser` | Information about the user that created the item. |
| `LastModifiedBy` | `GSuiteUser` | Information about the user that last modified the item. |

## Notes

- The `Uri` property is functionally identical to `Url` but is positioned last in display order. Prefer `Url` for new workflows.
- Specialized subclasses exist for Google Spreadsheets (`SpreadsheetRemoteItem`) and native Google file types (`GDriveGoogleFileRemoteItem`), but they are created internally via the factory method and expose the same public surface.
- The `LocalCopy` property lazily creates a `GDriveLocalItem` that handles downloading.

## Used By

Activities that return or accept this type -- see activity docs for details.
