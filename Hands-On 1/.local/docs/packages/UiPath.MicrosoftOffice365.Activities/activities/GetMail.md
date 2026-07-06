# Get Mail (Classic)

`UiPath.MicrosoftOffice365.Activities.Mail.GetMail`

Retrieves emails from a mail folder using the classic Office 365 activity interface. Supports OData query filtering and ordering.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Mail |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `Account` | Account | Input | `InArgument<string>` | No | | The account to use. |
| `MailFolder` | Mail Folder | Input | `InArgument<string>` | No | `Inbox` | The mail folder to retrieve emails from. |
| `EmailId` | Email ID | Options | `InArgument<string>` | No | | Ignores search criteria and returns an email with a specific ID. |
| `Query` | Query | Options | `InArgument<string>` | No | | An OData query to filter emails. |
| `Top` | Top | Options | `InArgument<int>` | No | | The maximum number of emails to retrieve. |
| `OnlyUnreadMessages` | Only Unread Messages | Options | `bool` | No | `True` | Retrieve only unread messages. |
| `OrderByDate` | Order By Date | Options | `EOrderByDate` | No | `NewestFirst` | Orders emails by received date. |
| `MarkAsRead` | Mark As Read | Options | `bool` | No | `False` | Whether to mark retrieved emails as read. |

## Output Properties

| Property | Display Name | Category | Type | Description |
|---|---|---|---|---|
| `Results` | Results | Output | `OutArgument<Office365Message[]>` | The retrieved email messages. |

## Output Model

See [`Office365Message`](types/Office365Message.md) for the email object structure.

## Enum Reference

| Enum | Values |
|---|---|
| `EOrderByDate` | `NewestFirst`, `OldestFirst` |

## XAML Example

```xml
<mail:GetMail
    DisplayName="Get Mail"
    MailFolder="Inbox"
    OnlyUnreadMessages="True"
    MarkAsRead="False" />
```

## Notes

- This is a classic activity. For new workflows, use `GetEmailListConnections` instead.
- Default mail folder is "Inbox".
