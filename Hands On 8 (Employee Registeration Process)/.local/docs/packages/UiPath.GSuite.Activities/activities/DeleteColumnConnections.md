# Delete Column

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.DeleteColumnConnections`

Deletes a column from an existing Google Sheets spreadsheet range.

**Package:** `UiPath.GSuite.Activities`
**Category:** Google Sheets
**Connector:** `uipath-google-sheets`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Spreadsheet | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The spreadsheet. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `Range` | Range | `InArgument` | `String` | Yes | | Indicates the range where to delete the column (e.g. `Sheet1!A1:E100`). |
| `HasHeaders` | Has Headers | `InArgument` | `Boolean` | | `true` | When true, you can specify the column by its header name. |
| `Column` | Column | `InArgument` | `String` | Yes | | The column name or column position to be deleted (e.g. `B` or `CustomerName`). |
| `DeleteMode` | Delete Mode | `InArgument` | [`ColumnDeleteMode`](#enum-reference) | | `Delete` | Indicates the delete behaviour of the selected column. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `RangeInformation` | Range Information | `OutArgument` | `RangeInformation` | The updated range information after the column is deleted. |

## Enum Reference

**`ColumnDeleteMode`** (`UiPath.GSuite.Activities.Sheets.Enums`): `Clear` (clear column values only), `Delete` (delete the entire column)

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
<gsuite:DeleteColumnConnections
    DisplayName="Delete Column"
    Range="[&quot;Sheet1!A1:E100&quot;]"
    HasHeaders="True"
    Column="[&quot;B&quot;]"
    DeleteMode="[ColumnDeleteMode.Delete]"
    RangeInformation="[rangeInfo]">
    <gsuite:DeleteColumnConnections.Item>
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
    </gsuite:DeleteColumnConnections.Item>
</gsuite:DeleteColumnConnections>
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- When `HasHeaders` is `true`, you can specify the column by its header name instead of the column letter.
- `Clear` mode clears the column values without removing the column. `Delete` mode removes the entire column.
