# Update File/Folder Permission

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.UpdateFileFolderPermissionConnections`

Updates a file permission for the specified file or folder in Google Drive.

**Package:** `UiPath.GSuite.Activities`
**Category:** Drive
**Connector:** `uipath-google-drive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | File or Folder | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The file or folder whose permission to update. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `PermissionId` | Permission ID | `InArgument` | `string` | Yes | | The ID of the permission to update. |
| `Role` | Role | `InArgument` | `Roles` | Yes | `READER` | The permission role to set. |
| `ExpirationTime` | Expiration Time | `InArgument` | `DateTime?` | No | | When the permission expires. Cannot be set for `OWNER`/`WRITER` roles or with `RemoveExpiration`. |
| `RemoveExpiration` | Remove Expiration | `InArgument` | `bool` | No | `false` | Whether to remove the expiration date. Cannot be used with `ExpirationTime`. |
| `UseDomainAdminAccess` | Use Domain Admin Access | `InArgument` | `bool` | No | `false` | Whether to act as a domain administrator. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google Workspace connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | [`GDriveItemPermission`](types/GDriveItemPermission.md) | The updated permission details. |

## Enum Reference

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
<gsuite:UpdateFileFolderPermissionConnections
    DisplayName="Update File/Folder Permission"
    ConnectionId="{x:Null}"
    PermissionId="[permissionId]"
    Role="[Roles.WRITER]"
    UseDomainAdminAccess="[False]"
    Result="[updatedPermission]">
    <gsuite:UpdateFileFolderPermissionConnections.Item>
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
    </gsuite:UpdateFileFolderPermissionConnections.Item>
</gsuite:UpdateFileFolderPermissionConnections>
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- Requires a Google Workspace connection with Drive scope.
- `PermissionId` and `Role` are required; the activity will fail validation if not provided.
- `ExpirationTime` cannot be set when `Role` is `OWNER` or `WRITER`.
- `ExpirationTime` and `RemoveExpiration` cannot both be set simultaneously.
