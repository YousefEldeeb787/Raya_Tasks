# Wait For Folder Created

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Activities.WaitForFolderCreated`

Pauses the workflow until a new folder matching the specified filter is created in Google Drive.

**Package:** `UiPath.GSuite.Activities`
**Category:** Drive
**Connector:** `uipath-google-drive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `DriveItemArgument` | Folder | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | No | | The parent folder to monitor for new folder creation. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `Filter` | Filter | `Property` | `TriggerFileFilterWithVariablesCollection` | No | | Conditions to filter which folder creation events to respond to. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google Workspace connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Folder | `OutArgument` | [`GDriveRemoteItem`](types/GDriveRemoteItem.md) | The newly created folder that triggered the wait. |
| `JobData` | Job Data | `OutArgument` | `JobInformation` | Metadata about the job that triggered this activity. |

## Output Model

Returns a [`GDriveRemoteItem`](types/GDriveRemoteItem.md) with folder ID, name, URL, MIME type, dates, and size.

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
<gsuite:WaitForFolderCreated
    DisplayName="Wait For Folder Created"
    ConnectionId="{x:Null}"
    Result="[newFolder]"
    JobData="[jobData]">
    <gsuite:WaitForFolderCreated.DriveItemArgument>
        <models:DriveItemArgument InputMode="UrlOrId">
            <models:DriveItemArgument.IdOrUrl>
                <InArgument x:TypeArguments="x:String">[parentFolderId]</InArgument>
            </models:DriveItemArgument.IdOrUrl>
          <models:DriveItemArgument.Backup>
    <usau:BackupSlot x:TypeArguments="driveEnums:EDriveItemMode" StoredValue="UrlOrId">
      <usau:BackupSlot.BackupValues>
        <scg:Dictionary x:TypeArguments="driveEnums:EDriveItemMode, scg:List(x:Object)" />
      </usau:BackupSlot.BackupValues>
    </usau:BackupSlot>
  </models:DriveItemArgument.Backup>
</models:DriveItemArgument>
    </gsuite:WaitForFolderCreated.DriveItemArgument>
</gsuite:WaitForFolderCreated>
```

## Notes

- This is a persistence activity -- the workflow suspends and resumes when the trigger condition is met.
- Uses `ObjectName` of `Folder` to filter for folder creation events only.
- Requires a Google Workspace connection with Drive scope.
