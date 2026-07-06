# Get Email List

`UiPath.MicrosoftOffice365.Activities.Mail.GetEmailListConnections`

Gets the list of emails within a mail folder. Supports filtering by read status, attachments, importance, and includes options for subfolder inclusion and marking retrieved emails as read.

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
| `IncludeSubfolders` | Include Subfolders | Input | `InArgument<bool>` | No | `False` | Include subfolders or not. |
| `MaxResults` | Max Results | Options | `InArgument<int>` | No | `100` | The maximum number of emails to retrieve. Values less than or equal to zero return all results. |
| `UnreadOnly` | Unread Only | Options | `InArgument<bool>` | No | `False` | Retrieve only unread emails. |
| `WithAttachmentsOnly` | With Attachments Only | Options | `InArgument<bool>` | No | `False` | Retrieve only emails with attachments. |
| `MarkAsRead` | Mark As Read | Options | `InArgument<bool>` | No | `False` | Indicates whether to mark retrieved emails as read after fetching them. |
| `Importance` | Importance | Options | `InArgument<ImportanceFilter>` | No | `Any` | Filter by email importance. |
| `Filter` | Filter | | [`MailFilterCollection`](filtering/MailFilterCollection.md) | | | Condition-based filter. See [MailFilterCollection](filtering/MailFilterCollection.md). |

## Output Properties

| Property | Display Name | Category | Type | Description |
|---|---|---|---|---|
| `EmailList` | Email List | Output | `OutArgument<List<`[`Office365Message`](types/Office365Message.md)`>>` | The processed email list. |

## Output Model

See [`Office365Message`](types/Office365Message.md) for the email object structure.

## Enum Reference

| Enum | Values |
|---|---|
| `ImportanceFilter` | `Any`, `Low`, `Normal`, `High` |

## XAML Example

```xml
<mail:GetEmailListConnections
    DisplayName="Get Email List"
    MaxResults="50"
    UnreadOnly="True"
    MarkAsRead="False" />
```

## Notes

- Default MaxResults is 100. Set to 0 or a negative value to retrieve all emails.
- The activity supports server-side filtering via condition builder, free text, or OData query.
