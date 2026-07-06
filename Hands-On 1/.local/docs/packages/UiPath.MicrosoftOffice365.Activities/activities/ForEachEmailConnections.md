# For Each Email

`UiPath.MicrosoftOffice365.Activities.Mail.ForEachEmailConnections`

Iterates over a collection of emails in a specified mail folder. Supports filtering by read status, attachments, importance, and includes options for subfolder inclusion and marking emails as read during iteration.

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
| `MaxResults` | Max Results | Options | `InArgument<int>` | No | `100` | The maximum number of emails to iterate over. Values less than or equal to zero return all results. |
| `UnreadOnly` | Unread Only | Options | `InArgument<bool>` | No | `False` | Retrieve only unread emails. |
| `WithAttachmentsOnly` | With Attachments Only | Options | `InArgument<bool>` | No | `False` | Retrieve only emails with attachments. |
| `MarkAsRead` | Mark As Read | Options | `InArgument<bool>` | No | `False` | Indicates whether to mark each email as read during iteration. |
| `Importance` | Importance | Options | `InArgument<ImportanceFilter>` | No | `Any` | Filter by email importance. |
| `Filter` | Filter | | [`MailFilterCollection`](filtering/MailFilterCollection.md) | | | Condition-based filter. See [MailFilterCollection](filtering/MailFilterCollection.md). |

## Output Properties

| Property | Display Name | Category | Type | Description |
|---|---|---|---|---|
| `Length` | Length | Output | `OutArgument<int>` | The number of emails processed. |

## Output Model

Each iteration provides a [`Office365Message`](types/Office365Message.md) as `CurrentEmail` and an `int` as `CurrentEmailIndex`.

## Enum Reference

| Enum | Values |
|---|---|
| `ImportanceFilter` | `Any`, `Low`, `Normal`, `High` |

## XAML Example

```xml
<mail:ForEachEmailConnections
    DisplayName="For Each Email"
    MaxResults="50"
    UnreadOnly="True"
    MarkAsRead="False" />
```

## Notes

- The `Body` property contains a sequence of activities executed for each email in the iteration.
- Current item variable name defaults to `CurrentEmail` and current index to `CurrentEmailIndex`.
