# Read Range

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.ReadRangeConnections`

Reads a range from a Google Sheets spreadsheet into a `DataTable`.

**Package:** `UiPath.GSuite.Activities`
**Category:** Google Sheets
**Connector:** `uipath-google-sheets`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Spreadsheet | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The spreadsheet. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `Range` | Range | `InArgument` | `String` | Yes | | The spreadsheet range (e.g. `Sheet1!A1:D10`). |
| `HasHeaders` | Has Headers | `InArgument` | `Boolean` | | `true` | Whether the first row of the range contains column headers. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ReadAs` | Read As | [`ValuesType`](#enum-reference) | `Values` | The type of the values to read. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | `DataTable` | The result as a `DataTable`. |
| `RangeInformation` | Range Information | `OutArgument` | `RangeInformation` | The detailed range information. |

## Enum Reference

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
<gsuite:ReadRangeConnections
    x:TypeArguments="sd:DataTable"
    DisplayName="Read Range"
    Range="[&quot;Sheet1!A1:D10&quot;]"
    HasHeaders="True"
    ReadAs="[ValuesType.Values]"
    Result="[dataTable]"
    RangeInformation="[rangeInfo]">
    <gsuite:ReadRangeConnections.Item>
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
    </gsuite:ReadRangeConnections.Item>
</gsuite:ReadRangeConnections>
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- The `ReadRangeConnections` class is generic (`ReadRangeConnections<T>` where `T : DataTable`). Use `x:TypeArguments` to specify the output type.
- The `Range` property uses the combined format `SheetName!RangeAddress` (e.g. `Sheet1!A1:D10`).
- Column names are validated at design time when `HasHeaders` is `true`.
