# Get Form Response List

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Activities.GetFormResponseList`

Retrieves a list of responses submitted to a Google Form, optionally filtered by submission time.

**Package:** `UiPath.GSuite.Activities`
**Category:** Forms
**Connector:** `uipath-google-forms`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Form` | Form | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The Google Form to retrieve responses from. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `SubmittedAfter` | Submitted After | `InArgument` | `DateTime` | No | | Only return form responses submitted after this date and time. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google Workspace connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Responses | `OutArgument` | `IList<GFormResponse>` | The list of form responses matching the filter. |

## Output Model

Returns a list of `GFormResponse` items.

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
<gsuite:GetFormResponseList
    DisplayName="Get Form Responses"
    ConnectionId="{x:Null}"
    SubmittedAfter="[DateTime.Today.AddDays(-7)]"
    Result="[responses]">
    <gsuite:GetFormResponseList.Form>
        <models:DriveItemArgument InputMode="UrlOrId">
            <models:DriveItemArgument.IdOrUrl>
                <InArgument x:TypeArguments="x:String">[formIdOrUrl]</InArgument>
            </models:DriveItemArgument.IdOrUrl>
          <models:DriveItemArgument.Backup>
    <usau:BackupSlot x:TypeArguments="driveEnums:EDriveItemMode" StoredValue="UrlOrId">
      <usau:BackupSlot.BackupValues>
        <scg:Dictionary x:TypeArguments="driveEnums:EDriveItemMode, scg:List(x:Object)" />
      </usau:BackupSlot.BackupValues>
    </usau:BackupSlot>
  </models:DriveItemArgument.Backup>
</models:DriveItemArgument>
    </gsuite:GetFormResponseList.Form>
</gsuite:GetFormResponseList>
```

## Notes

- Requires one of the following OAuth scopes: `forms.responses.readonly`.
- Use `SubmittedAfter` to narrow results to recent responses.
