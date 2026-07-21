# GDriveItemPermission

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Drive.Models.GDriveItemPermission`

Represents a permission entry on a Google Drive file or folder. Each permission describes who has access and what level of access they have.

**Assembly:** `UiPath.GSuite`

## Properties

| Property | Type | Description |
|----------|------|-------------|
| `Id` | `string` | The unique identifier of the permission. |
| `DisplayName` | `string` | The display name of the grantee. |
| `Role` | `string` | The role granted (e.g., "owner", "organizer", "fileOrganizer", "writer", "commenter", "reader"). |
| `Type` | `string` | The type of grantee (e.g., "user", "group", "domain", "anyone"). |
| `View` | `PermissionView?` | The view associated with the permission (Published or Metadata). |
| `AllowFileDiscovery` | `bool?` | Whether the permission allows the file to be discovered through search. Only applicable for "domain" or "anyone" type. |
| `Deleted` | `bool?` | Whether the account associated with this permission has been deleted. |
| `Domain` | `string` | The domain to which this permission refers (for "domain" type permissions). |
| `EmailAddress` | `string` | The email address of the user or group to which this permission refers. |
| `ExpirationTime` | `DateTimeOffset?` | The time at which this permission will expire. |
| `Kind` | `string` | The resource kind (typically "drive#permission"). |
| `PendingOwner` | `bool?` | Whether the account is a pending owner. |
| `PhotoLink` | `string` | A link to the user's profile photo. |
| `PermissionDetails` | `IEnumerable<GDriveItemPermissionData>` | Detailed information about the permission, including inheritance data. |

## Related Types

### GDriveItemPermissionData

| Property | Type | Description |
|----------|------|-------------|
| `Inherited` | `bool?` | Whether this permission is inherited from a parent folder. |
| `InheritedFrom` | `string` | The ID of the item from which this permission is inherited. |
| `Role` | `string` | The role for this permission detail. |
| `PermissionType` | `GDriveItemPermissionType` | Whether this is a file-level or member-level permission. |

### GDriveItemPermissionType Enum

| Value | Description |
|-------|-------------|
| `File` | File-level permission. |
| `Member` | Member-level permission. |

### PermissionView Enum

| Value | Description |
|-------|-------------|
| `Published` | Published view. |
| `Metadata` | Metadata-only view. |

## Used By

Activities that return or accept this type -- see activity docs for details.
