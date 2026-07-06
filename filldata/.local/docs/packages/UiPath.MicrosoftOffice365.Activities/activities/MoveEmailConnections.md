# Move Email

`UiPath.MicrosoftOffice365.Activities.Mail.MoveEmailConnections`

Moves an email to a folder within the same mailbox.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Mail |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `Email` | Email | Input | [`InArgument<Office365Message>`](types/Office365Message.md) | Yes | | The email to move. |
| `MailFolderArgument` | Destination Folder | | [`MailFolderArgument`](components/MailFolderArgument.md) | No | | Specifies the destination mail folder. See [MailFolderArgument](components/MailFolderArgument.md) for input modes. |
| `MailboxArg` | Mailbox | | [`MailboxArgument`](components/MailboxArgument.md) | No | | Specifies the mailbox to use. See [MailboxArgument](components/MailboxArgument.md) for input modes. |
| `CreateFolderIfMissing` | Create Folder If Missing | Options | `InArgument<bool>` | No | `False` | Whether to create the folder if it is missing. |

## Output Properties

| Property | Display Name | Category | Type | Description |
|---|---|---|---|---|
| `Result` | Moved Email | Output | [`OutArgument<Office365Message>`](types/Office365Message.md) | The moved email. |

## Output Model

See [`Office365Message`](types/Office365Message.md) for the email object structure.

## XAML Example

```xml
<mail:MoveEmailConnections
    DisplayName="Move Email"
    Email="{x:Null}" />
```

## Notes

- The `Email` property is required.
- Use `CreateFolderIfMissing` to automatically create the destination folder when using manual path entry.
