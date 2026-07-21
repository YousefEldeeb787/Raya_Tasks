# Write Range

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.WriteRangeConnections`

Writes a `DataTable` to a range in a Google Sheets spreadsheet.

**Package:** `UiPath.GSuite.Activities`
**Category:** Google Sheets
**Connector:** `uipath-google-sheets`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Spreadsheet | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The spreadsheet. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `Range` | Range | `InArgument` | `String` | Yes | | The spreadsheet range (e.g. `Sheet1!A1`). |
| `Source` | Data | `InArgument` | `DataTable` | Yes | | The `DataTable` that will be written in the spreadsheet. |
| `IncludeHeaders` | Include Headers | `InArgument` | `Boolean` | | `true` | If checked, the `DataTable` headers are also inserted into the spreadsheet. |
| `WriteMode` | Write Mode | `InArgument` | [`RangeWriteMode`](#enum-reference) | | `Overwrite` | Indicates how to add the `DataTable` to the indicated range. |
| `RowPosition` | Row Position | `InArgument` | `Int32` | | `0` | The row index position where the activity inserts rows. Used when `WriteMode` is `Insert`. |

## Enum Reference

**`RangeWriteMode`** (`UiPath.GSuite.Activities.Sheets.Enums`): `Overwrite` (overwrite any previous data), `Append` (append to the bottom), `AppendRight` (append to the right), `Insert` (insert at a chosen row index), `InsertRight` (insert at a chosen column index)

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
<gsuite:WriteRangeConnections
    DisplayName="Write Range"
    Range="[&quot;Sheet1!A1&quot;]"
    Source="[dataTable]"
    IncludeHeaders="True"
    WriteMode="[RangeWriteMode.Overwrite]">
    <gsuite:WriteRangeConnections.Item>
        <models:DriveItemArgument InputMode="UrlOrId">
            <models:DriveItemArgument.IdOrUrl>
                <InArgument x:TypeArguments="x:String">[spreadsheetId]</InArgument>
            </models:DriveItemArgument.IdOrUrl>
          <models:DriveItemArgument.Backup>
    <usau:BackupSlot x:TypeArguments="driveEnums:EDriveItemMode" StoredValue="UrlOrId">
      <usau:BackupSlot.BackupValues>
        <scg:Dictionary x:TypeArguments="driveEnums:EDriveItemMode, scg:List(x:Object)" />
      </usau:BackupSlot.BackupValues>
    </usau:BackupSlot>
  </models:DriveItemArgument.Backup>
</models:DriveItemArgument>
    </gsuite:WriteRangeConnections.Item>
</gsuite:WriteRangeConnections>
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- The `Range` property uses the combined format `SheetName!RangeAddress`.
- `Overwrite` replaces existing content. `Append` adds rows below. `AppendRight` adds columns to the right.
- `Insert` inserts rows at the specified `RowPosition`. `InsertRight` inserts columns at the specified position.
