# Send Draft Email

`UiPath.MicrosoftOffice365.Activities.Mail.SendDraftEmailConnections`

Sends a previously saved draft email message.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Mail |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `MessageId` | Message ID | Input | `InArgument<string>` | Yes | | The ID of the draft message to send. |
| `MailboxArg` | Mailbox | | [`MailboxArgument`](components/MailboxArgument.md) | No | | Specifies the mailbox to use. See [MailboxArgument](components/MailboxArgument.md) for input modes. |

## Output Model

This activity does not return an output object.

## XAML Example

```xml
<mail:SendDraftEmailConnections
    DisplayName="Send Draft Email"
    MessageId="{x:Null}" />
```

## Notes

- The `MessageId` property is required and must reference an existing draft.
