# Insert Image

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.InsertImageConnections`

Inserts an image into a Google Docs document at a specified location. The image can be a Google Drive file or any publicly accessible URL.

**Package:** `UiPath.GSuite.Activities`
**Category:** Google Docs
**Connector:** `uipath-google-docs`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Document | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The target Google Docs document. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `ImageItem` | Image | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The image to insert. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `InsertImageMode` | Location | `Property` | `InsertImageMode` | Yes | `BeginningOfDocument` | Where to insert the image in the document. |
| `Section` | Section | `InArgument` | `string` | Conditional | | The section heading to use as a reference point. Required when using a section-based mode. |
| `MatchCase` | Match Case | `Property` | `bool` | No | `false` | Whether section matching should be case-sensitive. |
| `MatchMode` | Match Mode | `Property` | `MatchMode` | No | `Contains` | How to match the section text: Contains or Equals. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google Workspace connection to use. |

## Enum Reference

### `InsertImageMode`
| Value | Description |
|-------|-------------|
| `BeginningOfDocument` | Inserts the image at the very beginning of the document. |
| `EndOfDocument` | Appends the image at the end of the document. |
| `BeginningOfSection` | Inserts the image at the beginning of a named section. Requires `Section`. |
| `EndOfSection` | Inserts the image at the end of a named section. Requires `Section`. |

### `MatchMode`
| Value | Description |
|-------|-------------|
| `Contains` | Section heading contains the specified text |
| `Equals` | Section heading exactly equals the specified text |

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
<gsuite:InsertImageConnections
    DisplayName="Insert Image"
    ConnectionId="{x:Null}"
    InsertImageMode="EndOfDocument">
    <gsuite:InsertImageConnections.Item>
        <models:DriveItemArgument InputMode="UrlOrId">
            <models:DriveItemArgument.IdOrUrl>
                <InArgument x:TypeArguments="x:String">[documentIdOrUrl]</InArgument>
            </models:DriveItemArgument.IdOrUrl>
          <models:DriveItemArgument.Backup>
    <usau:BackupSlot x:TypeArguments="driveEnums:EDriveItemMode" StoredValue="UrlOrId">
      <usau:BackupSlot.BackupValues>
        <scg:Dictionary x:TypeArguments="driveEnums:EDriveItemMode, scg:List(x:Object)" />
      </usau:BackupSlot.BackupValues>
    </usau:BackupSlot>
  </models:DriveItemArgument.Backup>
</models:DriveItemArgument>
    </gsuite:InsertImageConnections.Item>
    <gsuite:InsertImageConnections.ImageItem>
        <models:DriveItemArgument InputMode="UrlOrId">
            <models:DriveItemArgument.IdOrUrl>
                <InArgument x:TypeArguments="x:String">[imageUrl]</InArgument>
            </models:DriveItemArgument.IdOrUrl>
          <models:DriveItemArgument.Backup>
    <usau:BackupSlot x:TypeArguments="driveEnums:EDriveItemMode" StoredValue="UrlOrId">
      <usau:BackupSlot.BackupValues>
        <scg:Dictionary x:TypeArguments="driveEnums:EDriveItemMode, scg:List(x:Object)" />
      </usau:BackupSlot.BackupValues>
    </usau:BackupSlot>
  </models:DriveItemArgument.Backup>
</models:DriveItemArgument>
    </gsuite:InsertImageConnections.ImageItem>
</gsuite:InsertImageConnections>
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- This activity has no output properties -- it modifies the document in place.
- Requires one of the following OAuth scopes: `drive.file`, `documents`, or `drive`.
- External image URLs must be publicly accessible (no authentication required).
- `Section`, `MatchCase`, and `MatchMode` are only relevant when using a section-based `InsertImageMode`.
