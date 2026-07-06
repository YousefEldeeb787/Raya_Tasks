# New Email Received (Trigger)

`UiPath.MicrosoftOffice365.Activities.Mail.Triggers.NewEmailReceived`

Trigger that fires when a new email is received in the specified mail folder.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Mail |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `MailFolderArg` | Mail Folder | | [`TriggerMailFolderArgument`](components/TriggerMailFolderArgument.md) | No | | Specifies the mail folder to monitor. See [TriggerMailFolderArgument](components/TriggerMailFolderArgument.md) for input modes. |
| `MailboxArg` | Mailbox | | [`TriggerMailboxArgument`](components/TriggerMailboxArgument.md) | No | | Specifies the mailbox to use. See [TriggerMailboxArgument](components/TriggerMailboxArgument.md) for input modes. |
| `MarkAsRead` | Mark As Read | Options | `InArgument<bool>` | No | `False` | Indicates whether to mark the retrieved email as read. |
| `WithAttachmentsOnly` | With Attachments Only | Options | `bool` | No | `False` | Retrieve only emails with attachments. |
| `IncludeAttachments` | Include Attachments | Options | `InArgument<bool>` | No | `False` | Indicates if the email should contain the attachments. |

## Output Properties

| Property | Display Name | Category | Type | Description |
|---|---|---|---|---|
| `Result` | Received Email | Output | [`OutArgument<Office365Message>`](types/Office365Message.md) | The newly received email that triggered the activity. |

## Output Model

See [`Office365Message`](types/Office365Message.md) for the email object structure.

## XAML Example

```xml
<triggers:NewEmailReceived
    DisplayName="New Email Received"
    MarkAsRead="False" />
```

## Notes

- This is a trigger activity used to start workflows when a new email arrives.
- Supports filtering via the `Filter` property (condition builder).
- In debug mode, retrieves the most recent matching email.
