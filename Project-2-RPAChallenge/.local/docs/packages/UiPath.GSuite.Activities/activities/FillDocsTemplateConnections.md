# Fill Document Template

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.FillDocsTemplateConnections`

Inserts text into marked fields in a Google Docs document. Field markers are delimited by a configurable symbol (e.g. `{{FieldName}}`), and each field is replaced with the mapped value.

**Package:** `UiPath.GSuite.Activities`
**Category:** Docs
**Connector:** `uipath-google-docs`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Document | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The Google Docs document containing field markers. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `MappedFields` | Document fields | `Property` | `IDictionary<String, InArgument<String>>` | Yes | | The list of fields identified in the selected document and their replacement values. Each entry maps a field name to a string expression. |
| `Symbol` | Symbol | `InArgument` | `String` | Yes | `{{ }}` | The delimiter symbol used to identify document fields. Defaults to double curly brackets `{{FieldName}}`. |

The activity also supports an optional template document (via the "Use document template" designer menu action):

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `TemplateFileId` | Document template | `InArgument` | `String` | No | | The URL or ID of the template document. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google Workspace connection to use. |

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
<gsuite:FillDocsTemplateConnections
    DisplayName="Fill Document Template"
    ConnectionId="{x:Null}"
    Symbol="[symbolValue]">
    <gsuite:FillDocsTemplateConnections.Item>
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
    </gsuite:FillDocsTemplateConnections.Item>
    <gsuite:FillDocsTemplateConnections.MappedFields>
        <x:Reference>mappedFieldsDictionary</x:Reference>
    </gsuite:FillDocsTemplateConnections.MappedFields>
</gsuite:FillDocsTemplateConnections>
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- This activity has no output properties -- it modifies the document in place.
- A validation error is raised if `Symbol` is empty or has an invalid format. The symbol must consist of valid delimiter parts (e.g. `{{` and `}}`).
- A validation error is raised if `MappedFields` is null or empty.
- Field markers use the format: `{openDelimiter}FieldName{closeDelimiter}`, for example `{{CustomerName}}`.
- When using the browser to select a document, the designer automatically scans the document for field markers and pre-populates `MappedFields`.
