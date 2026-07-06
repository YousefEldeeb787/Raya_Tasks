# Wait For Email Sent

`UiPath.MicrosoftOffice365.Activities.WaitForEmailSent`

Suspends workflow execution and waits until an email is sent from the specified mailbox. This is a persistent (long-running) activity.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Mail |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `MailboxArg` | Mailbox | | [`MailboxArgument`](components/MailboxArgument.md) | No | | Specifies the mailbox to use. See [MailboxArgument](components/MailboxArgument.md) for input modes. |
| `WithAttachmentsOnly` | With Attachments Only | | `InArgument<bool>` | No | `False` | Retrieve only emails with attachments. |
| `IncludeAttachments` | Include Attachments | | `InArgument<bool>` | No | `False` | Indicates if the email should contain the attachments. |

## Output Model

The activity returns an [`Office365Message`](types/Office365Message.md) as its result.

See [`Office365Message`](types/Office365Message.md) for the email object structure.

## XAML Example

```xml
<activities:WaitForEmailSent
    DisplayName="Wait For Email Sent" />
```

## Notes

- This is a persistent activity that suspends workflow execution until the trigger fires.
- Supports filtering via the `Filter` property (condition builder with variables).
- In debug mode, retrieves the most recent matching sent email instead of waiting.
