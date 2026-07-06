# Email Sent (Trigger)

`UiPath.MicrosoftOffice365.Activities.Mail.Triggers.EmailSent`

Trigger that fires when an email is sent from the specified mailbox.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Mail |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `MailboxArg` | Mailbox | | [`TriggerMailboxArgument`](components/TriggerMailboxArgument.md) | No | | Specifies the mailbox to use. See [TriggerMailboxArgument](components/TriggerMailboxArgument.md) for input modes. |
| `WithAttachmentsOnly` | With Attachments Only | Options | `bool` | No | `False` | Retrieve only emails with attachments. |
| `IncludeAttachments` | Include Attachments | Options | `InArgument<bool>` | No | `False` | Indicates if the email should contain the attachments. |

## Output Properties

| Property | Display Name | Category | Type | Description |
|---|---|---|---|---|
| `Result` | Sent Email | Output | [`OutArgument<Office365Message>`](types/Office365Message.md) | The sent email that triggered the activity. |

## Output Model

See [`Office365Message`](types/Office365Message.md) for the email object structure.

## XAML Example

```xml
<triggers:EmailSent
    DisplayName="Email Sent" />
```

## Notes

- This is a trigger activity used to start workflows when an email is sent.
- Supports filtering via the `Filter` property (condition builder).
- In debug mode, retrieves the most recent matching sent email.
