# Forward Email

`UiPath.MicrosoftOffice365.Activities.Mail.ForwardEmailConnections`

Forwards an email to new recipients using Microsoft Office 365. Supports additional body text, custom subject, attachments, and delivery/read receipts.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Mail |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `Email` | Email | Input | [`InArgument<Office365Message>`](types/Office365Message.md) | Yes | | The email to forward. |
| `Body` | Body | Input | `InArgument<string>` | No | | The body of the forwarded email (HTML). |
| `NewSubject` | New Subject | Input | `InArgument<string>` | No | | The new subject. If left blank, the original subject is used. |
| `PreferredTimezone` | Preferred Timezone | Input | `InArgument<string>` | No | `UTC` | Preferred timezone for forwarded email messages. |
| `AttachmentInputMode` | Attachment Input Mode | Input | `AttachmentInputMode` | No | `Existing` | Determines how attachments are specified. |
| `Attachments` | Attachments | Input | `InArgument<IEnumerable<IResource>>` | No | | The attachments to be sent with the email. |
| `ArgumentAttachments` | Attachments (Builder) | Input | `IEnumerable<InArgument<IResource>>` | No | | Individual attachment arguments. |
| `AttachmentPaths` | Attachment Paths | Input | `InArgument<IEnumerable<string>>` | No | | The file paths of the attachments. |
| `ArgumentAttachmentPaths` | Attachment Paths (Builder) | Input | `IEnumerable<InArgument<string>>` | No | | The list of file paths of the attachments. |
| `To` | To | Recipients | `InArgument<IEnumerable<string>>` | Yes | | The primary recipients of the forwarded email. |
| `Cc` | CC | Recipients | `InArgument<IEnumerable<string>>` | No | | The secondary recipients. |
| `Bcc` | BCC | Recipients | `InArgument<IEnumerable<string>>` | No | | The hidden recipients. |
| `SaveAsDraft` | Save As Draft | Options | `InArgument<bool>` | No | `True` | Specifies whether the email should be saved as draft instead of being sent. |
| `IsDeliveryReceiptRequested` | Delivery Receipt | Options | `InArgument<bool>` | No | `False` | Specifies whether a delivery receipt was requested. |
| `IsReadReceiptRequested` | Read Receipt | Options | `InArgument<bool>` | No | `False` | Specifies whether a read receipt was requested. |
| `MailboxArg` | Forward As | | [`MailboxArgument`](components/MailboxArgument.md) | No | | The email address of a user or a shared mailbox to forward from. See [MailboxArgument](components/MailboxArgument.md) for input modes. |

## Output Model

This activity does not return an output object.

## Enum Reference

| Enum | Values |
|---|---|
| `AttachmentInputMode` | `Existing`, `Builder`, `FilePaths`, `FilePathsBuilder` |

## XAML Example

```xml
<mail:ForwardEmailConnections
    DisplayName="Forward Email"
    Email="{x:Null}"
    To="{x:Null}"
    SaveAsDraft="False" />
```

## Notes

- Both `Email` and `To` properties are required.
- The forwarded body is always treated as HTML.
- When `SaveAsDraft` is `True` (default), the forward is saved to Drafts instead of being sent.
