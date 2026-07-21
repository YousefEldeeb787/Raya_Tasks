# Add Sheet

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.AddSheetConnections`

Adds a new sheet to an existing Google Sheets spreadsheet.

**Package:** `UiPath.GSuite.Activities`
**Category:** Google Sheets
**Connector:** `uipath-google-sheets`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Spreadsheet | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The spreadsheet to add a sheet to. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `SheetName` | Sheet Name | `InArgument` | `String` | | | The name of the new sheet. If left blank, Google Sheets determines the name. |
| `PositionIndex` | Position Index | `InArgument` | `Int32` | | | The position of the new sheet within existing sheets. If empty, appended at the end. First sheet is index 0. |
| `RowCount` | Row Count | `InArgument` | `Int32` | | | The number of rows. Default is 1000. |
| `ColumnCount` | Column Count | `InArgument` | `Int32` | | | The number of columns. Default is 26 (A-Z). |
| `ConflictResolution` | Conflict Resolution | `InArgument` | [`ConflictBehavior`](#enum-reference) | | `Fail` | Conflict resolution when a sheet with the same name exists. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `NewSheetName` | New Sheet Name | `OutArgument` | `String` | The name of the created sheet. |

## Enum Reference

**`ConflictBehavior`** (`UiPath.GSuite.Activities.Drive.Enums`): `Replace` (replace existing), `Fail` (fail if exists)

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
<gsuite:AddSheetConnections
    DisplayName="Add Sheet"
    SheetName="[&quot;NewSheet&quot;]"
    PositionIndex="[0]"
    RowCount="[1000]"
    ColumnCount="[26]"
    ConflictResolution="[ConflictBehavior.Fail]"
    NewSheetName="[createdSheetName]">
    <gsuite:AddSheetConnections.Item>
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
    </gsuite:AddSheetConnections.Item>
</gsuite:AddSheetConnections>
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- `PositionIndex` must be non-negative.
- `RowCount` and `ColumnCount` must be positive when specified.
