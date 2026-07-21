# For Each Row

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.ForEachRowConnections`

Iterates over a collection of data rows in a Google Sheets range, executing the body for each row.

**Package:** `UiPath.GSuite.Activities`
**Category:** Google Sheets
**Connector:** `uipath-google-sheets`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Spreadsheet | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The spreadsheet. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `Range` | Range | `InArgument` | `String` | Yes | | The spreadsheet range (e.g. `Sheet1!A1:E100`). |
| `HasHeaders` | Has Headers | `InArgument` | `Boolean` | | `true` | Whether the range has headers. |
| `EmptyRowAction` | Empty Row Action | `InArgument` | [`RowIteratorAction`](#enum-reference) | | `Skip` | The action to take if an empty row is found. |
| `CurrentItemVariableName` | Current Item Variable Name | `Property` | `String` | Yes | `CurrentRow` | Identifier for the current `DataRow` in iteration. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ReadAs` | Read As | [`ValuesType`](#enum-reference) | `Values` | The type of the values to read. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Length` | Length | `OutArgument` | `Int32` | The number of rows processed. |

## Enum Reference

**`RowIteratorAction`** (`UiPath.GSuite.Activities.Sheets.Enums`): `Skip` (skip the empty row), `Process` (process the empty row), `Stop` (stop iteration)

**`ValuesType`** (`UiPath.GSuite.Sheets.Enums`): `Values`, `Formulas`, `Text`

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
    xmlns:sd="clr-namespace:System.Data;assembly=System.Data.Common"
-->
<gsuite:ForEachRowConnections
    x:TypeArguments="sd:DataRow"
    DisplayName="For Each Row"
    Range="[&quot;Sheet1!A1:E100&quot;]"
    HasHeaders="True"
    EmptyRowAction="[RowIteratorAction.Skip]"
    ReadAs="[ValuesType.Values]"
    Length="[rowCount]">
  <ActivityAction x:TypeArguments="sd:DataRow, x:Int32">
    <ActivityAction.Argument1>
      <DelegateInArgument x:TypeArguments="sd:DataRow" Name="CurrentRow">
    <gsuite:ForEachRowConnections.Item>
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
    </gsuite:ForEachRowConnections.Item>
</gsuite:ForEachRowConnections>
    </ActivityAction.Argument1>
    <ActivityAction.Argument2>
      <DelegateInArgument x:TypeArguments="x:Int32" Name="CurrentRowIndex">
    <gsuite:ForEachRowConnections.Item>
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
    </gsuite:ForEachRowConnections.Item>
</gsuite:ForEachRowConnections>
    </ActivityAction.Argument2>
    <Sequence DisplayName="Do">
      <!-- body activities here -->
    </Sequence>
  </ActivityAction>
</gsuite:ForEachRowConnections>
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- The `ForEachRowConnections` class is generic (`ForEachRowConnections<T>` where `T : DataRow`). Use `x:TypeArguments` in XAML.
- The body receives two arguments: the current `DataRow` and the current row index (`Int32`).
- Column names are validated at design time when `HasHeaders` is `true`.
- The `Range` property uses the combined format `SheetName!RangeAddress`.
