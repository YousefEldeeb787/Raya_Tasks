# Find and Replace Text

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.FindAndReplaceTextConnections`

Finds and replaces the specified text inside the body of a Google Docs document.

**Package:** `UiPath.GSuite.Activities`
**Category:** Docs
**Connector:** `uipath-google-docs`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Document | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The Google Docs document to find and replace text in. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `FindWhat` | Find | `InArgument` | `String` | Yes | | The text to be found in the document. |
| `ReplaceWith` | Replace with | `InArgument` | `String` | Yes | | The replacement text. |
| `MatchCase` | Match case | `InArgument` | `Boolean` | No | `false` | Specifies if the searched text should have the same case. |
| `ReplaceBehavior` | Replace | `Property` | [`ReplaceBehavior`](#replacebehavior) | No | `Once` | Specifies the replace behavior for the searched text. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google Workspace connection to use. |

## Enum Reference

### `ReplaceBehavior`

| Value | Description |
|-------|-------------|
| `Once` | Replaces only the first occurrence of the text. |
| `AllReccurences` | Replaces all occurrences of the text. |

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
<gsuite:FindAndReplaceTextConnections
    DisplayName="Find and Replace Text"
    ConnectionId="{x:Null}"
    FindWhat="[findText]"
    ReplaceWith="[replaceText]"
    MatchCase="False"
    ReplaceBehavior="Once">
    <gsuite:FindAndReplaceTextConnections.Item>
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
    </gsuite:FindAndReplaceTextConnections.Item>
</gsuite:FindAndReplaceTextConnections>
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- This activity has no output properties -- it modifies the document in place.
- Both `FindWhat` and `ReplaceWith` are required; a validation error is raised at design time if either has no expression.
- The replacement is performed across the entire document body. There is no option to restrict it to a section.
