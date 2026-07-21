# Clear File Label Fields

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.ClearFileLabelFieldsConnections`

Clears the values of specific fields on Google Drive labels applied to a file.

**Package:** `UiPath.GSuite.Activities`
**Category:** Drive
**Connector:** `uipath-google-drive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | File | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The file whose label fields to clear. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `LabelSelectionMode` | Label Selection Mode | `Property` | `ItemSelectionMode` | No | `MultiSelect` | How labels are specified: by multi-select in the designer or by a variable. |
| `SelectedLabels` | Labels | `Property` | `string` | Conditional | | The serialized list of labels selected via the designer. Required when `LabelSelectionMode` is `MultiSelect`. |
| `ManualEntrySelectedLabels` | Labels | `InArgument` | `IEnumerable<object>` | Conditional | | The labels as a variable collection of `GDriveLabel` objects. Required when `LabelSelectionMode` is `Variable`. |
| `FieldSelectionMode` | Field Selection Mode | `Property` | `ItemSelectionMode` | No | `MultiSelect` | How label fields are specified: by multi-select in the designer or by a variable. |
| `SelectedFields` | Fields | `Property` | `string` | Conditional | | The serialized list of label fields selected via the designer. Required when `FieldSelectionMode` is `MultiSelect`. |
| `ManualEntrySelectedFields` | Fields | `InArgument` | `IEnumerable<GDriveLabelField>` | Conditional | | The label fields to clear as a variable collection. Required when `FieldSelectionMode` is `Variable`. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google Workspace connection to use. |

## Enum Reference

### `ItemSelectionMode`
| Value | Description |
|-------|-------------|
| `MultiSelect` | Select via the designer multi-select widget |
| `Variable` | Provide as a runtime variable |

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
<gsuite:ClearFileLabelFieldsConnections
    DisplayName="Clear File Label Fields"
    ConnectionId="{x:Null}"
    LabelSelectionMode="Variable"
    ManualEntrySelectedLabels="[selectedLabels]"
    FieldSelectionMode="Variable"
    ManualEntrySelectedFields="[fieldsToClean]">
    <gsuite:ClearFileLabelFieldsConnections.Item>
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
    </gsuite:ClearFileLabelFieldsConnections.Item>
</gsuite:ClearFileLabelFieldsConnections>
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- Requires a Google Workspace connection with Drive and Drive Labels scopes.
- This activity clears field values (sets them to empty) rather than removing the label itself.
