# Write Row

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.WriteRowConnections`

Writes a single row of data to a Google Sheets spreadsheet.

**Package:** `UiPath.GSuite.Activities`
**Category:** Google Sheets
**Connector:** `uipath-google-sheets`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Spreadsheet | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The spreadsheet. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `Range` | Range | `InArgument` | `String` | Yes | | The spreadsheet range (e.g. `Sheet1!A1`). |
| `WriteMode` | Write Mode | `InArgument` | [`RangeWriteMode`](#enum-reference) | | `Append` | Indicates how to add the data to the indicated range. |
| `RowPosition` | Row Position | `InArgument` | `Int32` | | `0` | The row index position where the activity overwrites/inserts the row. |
| `DataRow` | Data Row | `InArgument` | `DataRow` | Conditional | | The data in a `DataRow` that will be written. Required when `DataType` is `DataRow`. |
| `ArrayRow` | Array Row | `InArgument` | `IEnumerable<Object>` | Conditional | | The data in an array that will be written. Required when `DataType` is `ArrayRow`. |
| `HasHeaders` | Has Headers | `InArgument` | `Boolean` | | `false` | Whether the range has headers (offsets the row position by 1 when true). |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `DataType` | Data Type | [`InputDataType`](#enum-reference) | `DataRow` | Indicates the type of data that will be written. |

## Enum Reference

**`RangeWriteMode`** (`UiPath.GSuite.Activities.Sheets.Enums`): `Overwrite` (overwrite any previous data), `Append` (append to the bottom), `AppendRight` (append to the right), `Insert` (insert at a chosen row index), `InsertRight` (insert at a chosen column index)

**`InputDataType`** (`UiPath.GSuite.Activities.Sheets.Enums`): `DataRow`, `ArrayRow`, `DataColumn`, `ArrayColumn`, `IndividualFields`

> **Supported `InputMode` values:** `UrlOrId` *(recommended for AI XAML)*, `UseExisting`, `FullPath`, `RelativePath`. `Browse` is available in Studio but requires interactive selection — not suitable for AI-generated XAML. `RelativePath` requires a parent folder browsed in Studio — also not suitable.

## XAML Example

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:gsuite="clr-namespace:UiPath.GSuite.Activities;assembly=UiPath.GSuite.Activities"
    xmlns:models="clr-namespace:UiPath.GSuite.Activities.Models;assembly=UiPath.GSuite.Activities"
    xmlns:driveEnums="clr-namespace:UiPath.GSuite.Activities.Drive.Enums;assembly=UiPath.GSuite.Activities"
    xmlns:usau="clr-namespace:UiPath.Shared.Activities.Utils;assembly=UiPath.GSuite.Activities"
    xmlns:scg="clr-namespace:System.Collections.Generic;assembly=mscorlib"
-->
<gsuite:WriteRowConnections
    DisplayName="Write Row"
    ConnectionId="{x:Null}"
    Range="[&quot;Sheet1!A1&quot;]"
    DataType="DataRow"
    DataRow="[myDataRow]"
    WriteMode="[RangeWriteMode.Append]">
    <gsuite:WriteRowConnections.Item>
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
    </gsuite:WriteRowConnections.Item>
</gsuite:WriteRowConnections>
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- When `DataType` is `IndividualFields`, the activity uses dynamic properties based on column headers discovered at design time.
- Write operations support batching when used inside a `ForEachRow` loop.
- When `HasHeaders` is `true`, the `RowPosition` is offset by 1 to account for the header row.
