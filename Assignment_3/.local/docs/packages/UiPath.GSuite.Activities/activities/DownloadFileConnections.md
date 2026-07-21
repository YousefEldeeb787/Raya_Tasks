# Download File

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.DownloadFileConnections`

Downloads a file from Google Drive to a local folder. Google Workspace files (Docs, Sheets, Slides, Drawings) are exported in the specified format.

**Package:** `UiPath.GSuite.Activities`
**Category:** Drive
**Connector:** `uipath-google-drive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | File | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The file to download. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `DestinationPath` | Destination Path | `InArgument` | `string` | No | | The local path where to save the downloaded file. |
| `ConflictResolution` | Conflict Resolution | `InArgument` | `ConflictBehavior` | No | `Fail` | Behavior when a file with the same name already exists locally. |
| `DownloadDocumentAs` | Download Document As | `InArgument` | `EGDocExportFormat` | No | `Word` | Export format for Google Docs documents. |
| `DownloadSpreadsheetAs` | Download Spreadsheet As | `InArgument` | `EGSheetExportFormat` | No | `Xlsx` | Export format for Google Sheets spreadsheets. |
| `DownloadPresentationAs` | Download Presentation As | `InArgument` | `EGSlideExportFormat` | No | `Ppt` | Export format for Google Slides presentations. |
| `DownloadDrawingAs` | Download Drawing As | `InArgument` | `EGDrawingExportFormat` | No | `Jpeg` | Export format for Google Drawings. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google Workspace connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `NewResult` | Result | `OutArgument` | `GDriveLocalItem` | The downloaded file as a local item reference. |

## Enum Reference

### `EGDocExportFormat`
| Value | Description |
|-------|-------------|
| `Word` | Microsoft Word (.docx) |
| `OpenDocument` | OpenDocument (.odt) |
| `RichText` | Rich Text (.rtf) |
| `Pdf` | PDF (.pdf) |
| `PlainText` | Plain Text (.txt) |
| `WebPage` | HTML (.html) |
| `EPub` | EPUB (.epub) |

### `EGSheetExportFormat`
| Value | Description |
|-------|-------------|
| `Xlsx` | Microsoft Excel (.xlsx) |
| `Ods` | OpenDocument (.ods) |
| `Pdf` | PDF (.pdf) |
| `WebPage` | HTML (.html) |
| `Csv` | Comma Separated Values (.csv) |
| `Tsv` | Tab Separated Values (.tsv) |

### `EGSlideExportFormat`
| Value | Description |
|-------|-------------|
| `Ppt` | Microsoft PowerPoint (.pptx) |
| `Odp` | OpenDocument (.odp) |
| `Pdf` | PDF (.pdf) |
| `PlainText` | Plain Text (.txt) |

### `EGDrawingExportFormat`
| Value | Description |
|-------|-------------|
| `Pdf` | PDF (.pdf) |
| `Jpeg` | JPEG (.jpg) |
| `Png` | PNG (.png) |
| `Svg` | SVG (.svg) |

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
<gsuite:DownloadFileConnections
    DisplayName="Download File"
    ConnectionId="{x:Null}"
    DestinationPath="[&quot;C:\Downloads&quot;]"
    DownloadDocumentAs="[EGDocExportFormat.Word]"
    DownloadSpreadsheetAs="[EGSheetExportFormat.Xlsx]"
    DownloadPresentationAs="[EGSlideExportFormat.Ppt]"
    DownloadDrawingAs="[EGDrawingExportFormat.Jpeg]"
    ConflictResolution="[ConflictBehavior.Fail]"
    NewResult="[localFile]">
    <gsuite:DownloadFileConnections.Item>
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
    </gsuite:DownloadFileConnections.Item>
</gsuite:DownloadFileConnections>
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- Requires a Google Workspace connection with Drive read scope.
- Export format properties only apply to native Google Workspace files. Regular files (PDF, images, etc.) are downloaded as-is.
