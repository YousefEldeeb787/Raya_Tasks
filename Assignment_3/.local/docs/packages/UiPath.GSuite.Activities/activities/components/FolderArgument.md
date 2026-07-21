# FolderArgument

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Activities.Gmail.Models.FolderArgument`

A composition component used by Gmail activities to specify a target Gmail folder (label). Supports browsing for a folder or manually entering a label name/path.

**Assembly:** `UiPath.GSuite.Activities`
**Inherits:** `BaseFolderArgument`

## InputMode (`FolderInputMode`)

Determines which properties are active.

| Mode | Value | Description | AI-XAML Suitable |
|------|-------|-------------|------------------|
| `Browse` | `0` | Select a folder from the remote browser in Studio. | **Not suitable for AI-generated XAML** -- requires interactive Studio UI. |
| `EnterPath` | `1` | Manually type a label name or path. | Yes |

## Properties

| Property | Type | Mode(s) | Description |
|----------|------|---------|-------------|
| `InputMode` | `FolderInputMode` | All | How the folder is specified: Browse to select from a list, or EnterPath to type a label name. |
| `BrowserFolderName` | `InArgument<string>` | Browse | The display name of the folder selected via the browser. |
| `BrowserFolderId` | `string` | Browse | The Gmail label ID persisted after the user selects a folder via the browser. |
| `ManualEntryFolderName` | `InArgument<string>` | EnterPath | The folder name/path entered manually by the user. |
| `ConnectionKey` | `string` | All | The connection identifier that was active when the folder was selected, used to detect connection changes. |
| `ConnectionDescriptor` | `string` | All | A human-readable label for the connection (e.g., account email) shown in the designer. |
| `Backup` | `BackupSlot<FolderInputMode>` | All | Stores the previous InputMode value so the designer can revert when switching modes. Studio **always** serializes this — every XAML example must include the `.Backup` child element. |

## XAML Examples

> **Supported `InputMode` values:** `EnterPath` *(only AI-suitable mode)*. `Browse` requires interactive Studio selection — not suitable for AI-generated XAML.

### EnterPath Mode (only AI-suitable mode)

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:gmailModels="clr-namespace:UiPath.GSuite.Activities.Gmail.Models;assembly=UiPath.GSuite.Activities"
    xmlns:gmailEnums="clr-namespace:UiPath.GSuite.Activities.Gmail.Enums;assembly=UiPath.GSuite.Activities"
    xmlns:usau="clr-namespace:UiPath.Shared.Activities.Utils;assembly=UiPath.GSuite.Activities"
    xmlns:scg="clr-namespace:System.Collections.Generic;assembly=mscorlib"
-->
<gmailModels:FolderArgument InputMode="EnterPath">
  <gmailModels:FolderArgument.ManualEntryFolderName>
    <InArgument x:TypeArguments="x:String">INBOX</InArgument>
  </gmailModels:FolderArgument.ManualEntryFolderName>
  <gmailModels:FolderArgument.Backup>
    <usau:BackupSlot x:TypeArguments="gmailEnums:FolderInputMode" StoredValue="EnterPath">
      <usau:BackupSlot.BackupValues>
        <scg:Dictionary x:TypeArguments="gmailEnums:FolderInputMode, scg:List(x:Object)" />
      </usau:BackupSlot.BackupValues>
    </usau:BackupSlot>
  </gmailModels:FolderArgument.Backup>
</gmailModels:FolderArgument>
```

## Notes

- The special label name "All Mail" is recognized and maps to the Gmail "All Mail" system label without requiring a lookup.
- When the connection changes in Browse mode, the folder is re-resolved by looking up the label by name.
- `BrowserFolderId` has the `[AutopilotIgnored]` attribute -- it is a regular property but is typically populated by the Studio browser, not by users directly.
- Every XAML example that contains a `FolderArgument` must include a `.Backup` child element with `StoredValue` matching the active `InputMode` and an empty `BackupValues` dictionary.

## Used By

Gmail activities that need a folder/label reference -- see activity docs for details.
