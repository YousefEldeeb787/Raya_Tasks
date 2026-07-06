# Turn On Automatic Replies

`UiPath.MicrosoftOffice365.Activities.Mail.TurnOnAutomaticRepliesConnections`

Activates and configures automatic replies (Out of Office) for the user's mailbox.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Mail |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `StartTime` | Start Time | Input | `InArgument<DateTimeOffset>` | No | | The date and time when the Out of Office starts. |
| `EndTime` | End Time | Input | `InArgument<DateTimeOffset>` | No | | The date and time when the Out of Office ends. |
| `InternalMessage` | Internal Message | Input | `InArgument<string>` | No | | The automatic replies message to be sent inside the organization. |
| `SendRepliesOutsideOrganization` | Send Replies Outside Organization | Input | `InArgument<bool>` | No | `False` | Whether automatic replies can be sent outside the organization. |
| `SendRepliesToContactsOnly` | Send Replies To Contacts Only | Input | `InArgument<bool>` | No | `False` | Whether automatic replies are sent only to contacts for external users. |
| `ExternalMessage` | External Message | Input | `InArgument<string>` | No | | The automatic replies message to be sent outside the organization. |

## Output Model

This activity does not return an output object.

## XAML Example

```xml
<mail:TurnOnAutomaticRepliesConnections
    DisplayName="Turn On Automatic Replies"
    InternalMessage="I am currently out of office." />
```

## Notes

- Requires the `MailboxSettings.ReadWrite` scope.
