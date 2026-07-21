# Read Text

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.ReadTextConnections`

Reads text from a Google Docs document. Can read all text or the content of a specific section.

**Package:** `UiPath.GSuite.Activities`
**Category:** Docs
**Connector:** `uipath-google-docs`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Document | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The Google Docs document to read from. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `ReadTextMode` | Text to read | `Property` | [`ReadTextMode`](#readtextmode) | Yes | `AllText` | What to read from the document: all text or a specific section. |
| `Section` | Section | `InArgument` | `String` | Conditional | | The section heading to read text from when `ReadTextMode` is `Section`. Required when `ReadTextMode` is `Section`. |
| `MatchCase` | Match case | `Property` | `Boolean` | No | `false` | Whether section matching should be case-sensitive. Visible only when `ReadTextMode` is `Section`. |
| `MatchMode` | Match mode | `Property` | [`MatchMode`](#matchmode) | No | `Contains` | How to match the section text: Contains or Equals. Visible only when `ReadTextMode` is `Section`. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google Workspace connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Text` | Text | `OutArgument` | `String` | The text content read from the document. |

## Enum Reference

### `ReadTextMode`

| Value | Description |
|-------|-------------|
| `AllText` | Reads all text from the document. |
| `Section` | Reads the content of a named section. |

### `MatchMode`

| Value | Description |
|-------|-------------|
| `Contains` | The section name contains the search value. |
| `Equals` | The section name exactly equals the search value. |

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
<gsuite:ReadTextConnections
    DisplayName="Read Text"
    ConnectionId="{x:Null}"
    ReadTextMode="AllText"
    Text="[docText]">
    <gsuite:ReadTextConnections.Item>
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
    </gsuite:ReadTextConnections.Item>
</gsuite:ReadTextConnections>
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- Requires one of the following OAuth scopes: `drive.file`, `documents`, or `drive`.
- `Section` is validated at design time -- a validation error is raised if `ReadTextMode` is `Section` and `Section` has no expression.
- `MatchCase` and `MatchMode` are only visible in the designer when `ReadTextMode` is `Section`.
