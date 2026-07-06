# Share Item

`UiPath.MicrosoftOffice365.Activities.Files.ShareItemConnections`

Shares a file or folder with the specified recipients via OneDrive or SharePoint.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Item | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The file or folder to share. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `RecipientType` | Recipient Type | `Property` | `GranteeType` | No | `PeopleInOrganization` | The type of recipient: `SpecificPeople`, `PeopleInOrganization`, `Anyone`. |
| `PermissionType` | Permission Type | `Property` | `GranteePermission` | No | `View` | The permission level: `View`, `Edit`. |
| `Recipients` | Recipients | `InArgument` | `IEnumerable<String>` | When SpecificPeople | | Email addresses of recipients. Required when RecipientType is `SpecificPeople`. |
| `Message` | Message | `InArgument` | `String` | No | | A message included in the sharing invitation email. |
| `SendInvitationEmail` | Send Invitation Email | `InArgument` | `Boolean` | No | `True` | Whether to send a sharing invitation email. |
| `SignInRequired` | Sign In Required | `InArgument` | `Boolean` | No | `False` | Whether the recipient must sign in to access the item. |
| `SharingLinkPassword` | Sharing Link Password | `InArgument` | `String` | No | | An optional password for the sharing link. |
| `ExpirationDate` | Expiration Date | `InArgument` | `DateTime` | No | | An optional expiration date for the sharing permission. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `AccessUrl` | Access URL | `OutArgument` | `String` | The web URL of the sharing link or drive item. |

## XAML Example

```xml
<o365files:ShareItemConnections DisplayName="Share Item" ConnectionId="[conn]"
    RecipientType="SpecificPeople" PermissionType="View" AccessUrl="[shareUrl]">
    <o365files:ShareItemConnections.Item>
        <models:DriveItemArgument ItemSelectionMode="UseExisting">
            <models:DriveItemArgument.Item>
                <InArgument x:TypeArguments="files:O365DriveRemoteItem">[fileToShare]</InArgument>
            </models:DriveItemArgument.Item>
        </models:DriveItemArgument>
    </o365files:ShareItemConnections.Item>
    <o365files:ShareItemConnections.Recipients>
        <InArgument x:TypeArguments="scg:IEnumerable(x:String)">[recipientEmails]</InArgument>
    </o365files:ShareItemConnections.Recipients>
</o365files:ShareItemConnections>
```
