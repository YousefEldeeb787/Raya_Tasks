# Read Cell

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.ReadCellConnections`

Reads a cell from an existing Google Sheets spreadsheet.

**Package:** `UiPath.GSuite.Activities`
**Category:** Google Sheets
**Connector:** `uipath-google-sheets`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Spreadsheet | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The spreadsheet. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `RangeLocation` | Sheet or Named Range | `InArgument` | `String` | Yes | | The spreadsheet sheet/named range containing the cell. |
| `Cell` | Cell | `InArgument` | `String` | | | The spreadsheet cell location (e.g. `A1`). Hidden when a named range is selected. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `WhatToRead` | What to Read | [`ValuesType`](#enum-reference) | `Values` | What to read (values, formulas, text). |
| `CellValueType` | Cell Value Type | [`CellFormatType`](#enum-reference) | `Text` | The type of value to be read. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | `T` | The cell retrieved data. |

## Enum Reference

**`ValuesType`** (`UiPath.GSuite.Sheets.Enums`): `Values`, `Formulas`, `Text`

**`CellFormatType`** (`UiPath.GSuite.Activities.Enums`): `General`, `DateAndTime`, `Date`, `Time`, `Number`, `Integer`, `Text`

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
<gsuite:ReadCellConnections
    x:TypeArguments="x:Object"
    DisplayName="Read Cell"
    RangeLocation="[&quot;Sheet1&quot;]"
    Cell="[&quot;A1&quot;]"
    WhatToRead="Values"
    CellValueType="Text"
    Result="[cellValue]">
    <gsuite:ReadCellConnections.Item>
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
    </gsuite:ReadCellConnections.Item>
</gsuite:ReadCellConnections>
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- The `ReadCellConnections` class is generic (`ReadCellConnections<T>`). Use `x:TypeArguments` in XAML to specify the output type.
- When `RangeLocation` is set to a named range, the `Cell` field is automatically hidden.
