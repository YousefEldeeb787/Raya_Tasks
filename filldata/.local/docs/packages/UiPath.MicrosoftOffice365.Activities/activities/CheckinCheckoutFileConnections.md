# Check In/Check Out File

`UiPath.MicrosoftOffice365.Activities.Files.CheckinCheckoutFileConnections`

Checks in or checks out a file in OneDrive or SharePoint.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | File | `InArgument` | [`O365DriveRemoteItem`](types/O365DriveRemoteItem.md) | Yes | | The file to check in or check out. |
| `ActionType` | Action Type | `InArgument` | `CheckinCheckoutActionType` | Yes | `Checkin` | The action to perform: `Checkin`, `Checkout`. |
| `Comment` | Comment | `InArgument` | `String` | No | | A check-in comment associated with the version. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

## XAML Example

```xml
<o365files:CheckinCheckoutFileConnections DisplayName="Check In File" ConnectionId="[conn]"
    ActionType="[CheckinCheckoutActionType.Checkin]" Comment="[comment]">
    <o365files:CheckinCheckoutFileConnections.Item>
        <InArgument x:TypeArguments="files:O365DriveRemoteItem">[file]</InArgument>
    </o365files:CheckinCheckoutFileConnections.Item>
</o365files:CheckinCheckoutFileConnections>
```
