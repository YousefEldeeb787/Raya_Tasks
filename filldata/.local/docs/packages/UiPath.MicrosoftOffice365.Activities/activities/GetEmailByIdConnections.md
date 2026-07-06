# Get Email By Id

`UiPath.MicrosoftOffice365.Activities.Mail.GetEmailByIdConnections`

Retrieves an email by its unique identifier using Microsoft Office 365 Integration Service.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Mail |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `EmailId` | Email ID | Input | `InArgument<string>` | Yes | | The unique identifier of the email. |
| `MailboxArg` | Mailbox | | [`MailboxArgument`](components/MailboxArgument.md) | No | | Specifies the mailbox to use. See [MailboxArgument](components/MailboxArgument.md) for input modes. |

## Output Properties

| Property | Display Name | Category | Type | Description |
|---|---|---|---|---|
| `Result` | Email | Output | [`OutArgument<Office365Message>`](types/Office365Message.md) | The email returned by the activity. |

## Output Model

See [`Office365Message`](types/Office365Message.md) for the email object structure.

## XAML Example

```xml
<mail:GetEmailByIdConnections
    DisplayName="Get Email By Id"
    EmailId="{x:Null}" />
```

## Notes

- The `EmailId` property is required.
- Supports shared mailbox via the `MailboxArg` property.
