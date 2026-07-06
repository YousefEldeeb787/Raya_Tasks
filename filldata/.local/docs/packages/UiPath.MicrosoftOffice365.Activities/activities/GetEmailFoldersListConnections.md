# Get Email Folders List

`UiPath.MicrosoftOffice365.Activities.Mail.GetEmailFoldersListConnections`

Retrieves the list of email folders for the selected account.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Mail |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `MailboxArg` | Mailbox | | [`MailboxArgument`](components/MailboxArgument.md) | No | | Specifies the mailbox to use. See [MailboxArgument](components/MailboxArgument.md) for input modes. |

## Output Model

The activity returns a `List<O365MailFolder>` containing the mail folders for the account.

## XAML Example

```xml
<mail:GetEmailFoldersListConnections
    DisplayName="Get Email Folders List" />
```

## Notes

- Supports shared mailbox via the `MailboxArg` property.
