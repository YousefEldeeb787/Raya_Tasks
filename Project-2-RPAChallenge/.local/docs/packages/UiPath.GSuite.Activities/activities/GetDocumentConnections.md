# Get Document Data

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.GetDocumentConnections`

Returns the data of a Google Docs document, resolving it by browse selection, URL/ID, relative path, full path, or an existing object.

**Package:** `UiPath.GSuite.Activities`
**Category:** Docs
**Connector:** `uipath-google-docs`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Document | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The Google Docs document to retrieve. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google Workspace connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Retrieved document data | `OutArgument` | `GoogleDocumentSlim` | The retrieved document data including metadata such as document ID, title, and structure. |

## Output Model

Returns a `GoogleDocumentSlim` with document ID, title, body content structure, and revision information.

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
<gsuite:GetDocumentConnections
    DisplayName="Get Document Data"
    ConnectionId="{x:Null}"
    Result="[docData]">
    <gsuite:GetDocumentConnections.Item>
        <models:DriveItemArgument InputMode="UrlOrId">
            <models:DriveItemArgument.IdOrUrl>
                <InArgument x:TypeArguments="x:String">[documentIdOrUrl]</InArgument>
            </models:DriveItemArgument.IdOrUrl>
          <models:DriveItemArgument.Backup>
    <usau:BackupSlot x:TypeArguments="driveEnums:EDriveItemMode" StoredValue="UrlOrId">
      <usau:BackupSlot.BackupValues>
        <scg:Dictionary x:TypeArguments="driveEnums:EDriveItemMode, scg:List(x:Object)" />
      </usau:BackupSlot.BackupValues>
    </usau:BackupSlot>
  </models:DriveItemArgument.Backup>
</models:DriveItemArgument>
    </gsuite:GetDocumentConnections.Item>
</gsuite:GetDocumentConnections>
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- Requires one of the following OAuth scopes: `drive.readonly`, `documents.readonly`, `drive`, `drive.file`, or `documents`.
- The returned `GoogleDocumentSlim` object can be passed to other Google Docs activities via the `UseExisting` mode.
