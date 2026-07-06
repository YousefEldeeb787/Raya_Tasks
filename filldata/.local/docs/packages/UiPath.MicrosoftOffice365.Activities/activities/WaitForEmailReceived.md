# Wait For Email Received

`UiPath.MicrosoftOffice365.Activities.WaitForEmailReceived`

Suspends workflow execution and waits until a new email is received in the specified mail folder. This is a persistent (long-running) activity.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Mail |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `MailFolderArgument` | Mail Folder | | [`MailFolderArgument`](components/MailFolderArgument.md) | No | | Specifies the mail folder to monitor for new emails. See [MailFolderArgument](components/MailFolderArgument.md) for input modes. |
| `MailboxArg` | Mailbox | | [`MailboxArgument`](components/MailboxArgument.md) | No | | Specifies the mailbox to use. See [MailboxArgument](components/MailboxArgument.md) for input modes. |
| `MarkAsRead` | Mark As Read | Options | `InArgument<bool>` | No | `False` | Indicates whether to mark the retrieved email as read. |
| `WithAttachmentsOnly` | With Attachments Only | | `InArgument<bool>` | No | `False` | Retrieve only emails with attachments. |
| `IncludeAttachments` | Include Attachments | | `InArgument<bool>` | No | `False` | Indicates if the email should contain the attachments. |

## Output Model

The activity returns an [`Office365Message`](types/Office365Message.md) as its result.

See [`Office365Message`](types/Office365Message.md) for the email object structure.

## XAML Example

```xml
<activities:WaitForEmailReceived
    DisplayName="Wait For Email Received"
    MarkAsRead="True" />
```

## Notes

- This is a persistent activity that suspends workflow execution until the trigger fires.
- Supports filtering via the `Filter` property (condition builder with variables).
- In debug mode, retrieves the most recent matching email instead of waiting.
