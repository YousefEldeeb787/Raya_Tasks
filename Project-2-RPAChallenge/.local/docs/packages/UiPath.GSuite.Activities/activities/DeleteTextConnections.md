# Delete Text

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.DeleteTextConnections`

Deletes text from a Google Docs document -- either a specific string of text or the entire content of a named section.

**Package:** `UiPath.GSuite.Activities`
**Category:** Docs
**Connector:** `uipath-google-docs`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Document | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The Google Docs document to delete text from. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `DeleteTextMode` | What to delete | `Property` | [`DeleteTextMode`](#deletetextmode) | Yes | `Text` | How to delete text from a Google Doc. |
| `Text` | Text | `InArgument` | `String` | Conditional | | The text to delete from the document. Required when `DeleteTextMode` is `Text`. |
| `Section` | Section | `InArgument` | `String` | Conditional | | The section to delete from the document. Required when `DeleteTextMode` is `Section`. |
| `MatchCase` | Match case | `InArgument` | `Boolean` | No | `false` | Specifies if the text to be deleted should match case. |
| `MatchMode` | Match mode | `Property` | [`MatchMode`](#matchmode) | No | `Contains` | Specifies how to find a matching section. Visible only when `DeleteTextMode` is `Section`. |
| `DeleteBehavior` | Delete | `Property` | [`ReplaceBehavior`](#replacebehavior) | No | `Once` | Whether to delete the first occurrence or all occurrences of the matched text. Visible only when `DeleteTextMode` is `Text`. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google Workspace connection to use. |

## Enum Reference

### `DeleteTextMode`

| Value | Description |
|-------|-------------|
| `Text` | Deletes the specified text from the document. |
| `Section` | Deletes the content of a section. |

### `ReplaceBehavior`

| Value | Description |
|-------|-------------|
| `Once` | Deletes only the first occurrence. |
| `AllReccurences` | Deletes all occurrences of the text. |

### `MatchMode`

| Value | Description |
|-------|-------------|
| `Contains` | Contains |
| `Equals` | Equals |

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
<gsuite:DeleteTextConnections
    DisplayName="Delete Text"
    ConnectionId="{x:Null}"
    DeleteTextMode="Text"
    Text="[textToDelete]"
    MatchCase="False"
    DeleteBehavior="AllReccurences">
    <gsuite:DeleteTextConnections.Item>
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
    </gsuite:DeleteTextConnections.Item>
</gsuite:DeleteTextConnections>
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- This activity has no output properties -- it modifies the document in place.
- `MatchCase` is typed as `InArgument<bool>` (expression-bindable), unlike the `Property<bool>` used in some sibling activities.
- `DeleteBehavior` and `MatchMode` are conditionally shown based on the selected `DeleteTextMode`.
