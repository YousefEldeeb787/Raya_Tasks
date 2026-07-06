# Get Newest Email

`UiPath.MicrosoftOffice365.Activities.Mail.GetNewestEmail`

Retrieves the newest email message from a mail folder. Supports filtering by read status, attachments, and importance.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Mail |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `MailFolderArgument` | Mail Folder | | [`MailFolderArgument`](components/MailFolderArgument.md) | No | | Specifies the mail folder to target. See [MailFolderArgument](components/MailFolderArgument.md) for input modes. |
| `MailboxArg` | Mailbox | | [`MailboxArgument`](components/MailboxArgument.md) | No | | Specifies the mailbox to use. See [MailboxArgument](components/MailboxArgument.md) for input modes. |
| `UnreadOnly` | Unread Only | Options | `InArgument<bool>` | No | `False` | Retrieve only unread emails. |
| `WithAttachmentsOnly` | With Attachments Only | Options | `InArgument<bool>` | No | `False` | Retrieve only emails with attachments. |
| `MarkAsRead` | Mark As Read | Options | `InArgument<bool>` | No | `False` | Indicates whether to mark the retrieved email as read after fetching it. |
| `Importance` | Importance | Options | `InArgument<ImportanceFilter>` | No | `Any` | Filter by email importance. |
| `Filter` | Filter | | [`MailFilterCollection`](filtering/MailFilterCollection.md) | | | Condition-based filter. See [MailFilterCollection](filtering/MailFilterCollection.md). |

## Output Properties

| Property | Display Name | Category | Type | Description |
|---|---|---|---|---|
| `Result` | Email | Output | [`OutArgument<Office365Message>`](types/Office365Message.md) | The newest email matching the criteria. |

## Output Model

See [`Office365Message`](types/Office365Message.md) for the email object structure.

## Enum Reference

| Enum | Values |
|---|---|
| `ImportanceFilter` | `Any`, `Low`, `Normal`, `High` |

## XAML Example

```xml
<mail:GetNewestEmail
    DisplayName="Get Newest Email"
    UnreadOnly="True"
    MarkAsRead="False" />
```

## Notes

- Throws an exception if no email matches the specified criteria.
- The activity supports server-side filtering via condition builder, free text, or OData query.
