# Share File/Folder

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.ShareFileFolderConnections`

Shares a file or folder with specified recipients by creating a permission in Google Drive.

**Package:** `UiPath.GSuite.Activities`
**Category:** Drive
**Connector:** `uipath-google-drive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | File or Folder | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The file or folder to share. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `ShareWith` | Share With | `Property` | `GranteeType` | No | `USER` | The type of grantee to share with. |
| `Role` | Role | `InArgument` | `Roles` | No | `READER` | The permission role to grant. |
| `UserEmail` | User Email | `InArgument` | `string` | Conditional | | Email address. Required when `ShareWith` is `USER` or `GROUP`. |
| `Domain` | Domain | `InArgument` | `string` | Conditional | | Domain name. Required when `ShareWith` is `DOMAIN`. |
| `SendNotificationEmail` | Send Notification Email | `InArgument` | `bool` | No | `true` | Whether to send a notification email to the grantee. |
| `UseDomainAdminAccess` | Use Domain Admin Access | `InArgument` | `bool` | No | `false` | Whether to issue the request as a domain administrator. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google Workspace connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `AccessUrl` | Access URL | `OutArgument` | `string` | The web URL of the shared drive item. |

## Enum Reference

### `GranteeType`
| Value | Description |
|-------|-------------|
| `USER` | Share with a specific user by email |
| `GROUP` | Share with a Google Group by email |
| `DOMAIN` | Share with all users in a domain |
| `ANYONE` | Share with anyone (public) |

### `Roles`
| Value | Description |
|-------|-------------|
| `OWNER` | Full ownership |
| `WRITER` | Edit access |
| `COMMENTER` | Comment-only access |
| `READER` | View-only access |

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
<gsuite:ShareFileFolderConnections
    DisplayName="Share File/Folder"
    ConnectionId="{x:Null}"
    ShareWith="USER"
    Role="[Roles.READER]"
    UserEmail="[&quot;colleague@example.com&quot;]"
    SendNotificationEmail="[True]"
    UseDomainAdminAccess="[False]"
    AccessUrl="[shareUrl]">
    <gsuite:ShareFileFolderConnections.Item>
        <models:DriveItemArgument InputMode="UrlOrId">
            <models:DriveItemArgument.IdOrUrl>
                <InArgument x:TypeArguments="x:String">[fileIdOrUrl]</InArgument>
            </models:DriveItemArgument.IdOrUrl>
          <models:DriveItemArgument.Backup>
    <usau:BackupSlot x:TypeArguments="driveEnums:EDriveItemMode" StoredValue="UrlOrId">
      <usau:BackupSlot.BackupValues>
        <scg:Dictionary x:TypeArguments="driveEnums:EDriveItemMode, scg:List(x:Object)" />
      </usau:BackupSlot.BackupValues>
    </usau:BackupSlot>
  </models:DriveItemArgument.Backup>
</models:DriveItemArgument>
    </gsuite:ShareFileFolderConnections.Item>
</gsuite:ShareFileFolderConnections>
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- Requires a Google Workspace connection with Drive scope.
- When `ShareWith` is `USER` or `GROUP`, `UserEmail` is required.
- When `ShareWith` is `DOMAIN`, `Domain` is required.
