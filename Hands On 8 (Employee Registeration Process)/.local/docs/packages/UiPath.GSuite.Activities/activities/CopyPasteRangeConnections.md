# Copy/Paste Range

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.CopyPasteRangeConnections`

Copies a range of cells and pastes it to a destination in the same or a different sheet within the same spreadsheet.

**Package:** `UiPath.GSuite.Activities`
**Category:** Google Sheets
**Connector:** `uipath-google-sheets`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Spreadsheet | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The spreadsheet. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `SourceSheetName` | Source Sheet Name | `InArgument` | `String` | Yes | | The name of the sheet containing the source range. |
| `SourceRange` | Source Range | `InArgument` | `String` | Yes | `A1:B2` | The range to copy (e.g. `A1:D10`). |
| `DestinationSheetName` | Destination Sheet Name | `InArgument` | `String` | | | The name of the destination sheet. Defaults to the source sheet if empty. |
| `DestinationStartingCell` | Destination Starting Cell | `InArgument` | `String` | Yes | `C3` | The starting cell where to paste the information (e.g. `C3`). Must be a valid single cell reference. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `PasteType` | Paste Type | [`PasteType`](#enum-reference) | `PASTE_NORMAL` | The type of the paste operation. |
| `PasteOrientation` | Paste Orientation | [`PasteOrientation`](#enum-reference) | `NORMAL` | The paste orientation. |

## Enum Reference

**`PasteType`** (`UiPath.GSuite.Activities.Enums`): `PASTE_NORMAL` (values, formulas, formats, and merges), `PASTE_VALUES` (values only), `PASTE_FORMAT` (format and data validation only), `PASTE_NO_BORDERS` (like normal but without borders), `PASTE_FORMULA` (formulas only), `PASTE_DATA_VALIDATION` (data validation only), `PASTE_CONDITIONAL_FORMATTING` (conditional formatting rules only)

**`PasteOrientation`** (`UiPath.GSuite.Activities.Enums`): `NORMAL` (paste normally), `TRANSPOSE` (rows become columns and vice versa)

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
<gsuite:CopyPasteRangeConnections
    DisplayName="Copy/Paste Range"
    SourceSheetName="[&quot;Sheet1&quot;]"
    SourceRange="[&quot;A1:D10&quot;]"
    DestinationSheetName="[&quot;Sheet2&quot;]"
    DestinationStartingCell="[&quot;A1&quot;]"
    PasteType="PASTE_VALUES"
    PasteOrientation="NORMAL">
    <gsuite:CopyPasteRangeConnections.Item>
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
    </gsuite:CopyPasteRangeConnections.Item>
</gsuite:CopyPasteRangeConnections>
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- If `DestinationSheetName` is not provided, the paste destination defaults to the same sheet as the source.
- The `DestinationStartingCell` must be a valid single cell reference.
