# Send Mail

`UiPath.MicrosoftOffice365.Activities.Mail.SendMailConnections`

Sends an email message using Microsoft Office 365 via Integration Service. Supports HTML or plain-text body, attachments, importance settings, delivery/read receipts, and sensitivity labels. Can also save as draft instead of sending.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Mail |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `To` | To | Recipients | `InArgument<IEnumerable<string>>` | Yes | | The main recipients of the email, separated by semicolon. |
| `Cc` | CC | Recipients | `InArgument<IEnumerable<string>>` | No | | The secondary recipients of the email, separated by semicolon. |
| `Bcc` | BCC | Recipients | `InArgument<IEnumerable<string>>` | No | | The hidden recipients of the email, separated by semicolon. |
| `Subject` | Subject | Email | `InArgument<string>` | No | | The subject of the email. |
| `Body` | Body | Email | `InArgument<string>` | No | | The body of the email in HTML format. Visible when InputType is HTML. |
| `TextBody` | Body | Email | `InArgument<string>` | No | | The body of the email in TEXT format. Visible when InputType is TEXT. |
| `InputType` | Input Type | Email | `BodyInputType` | No | `HTML` | Specifies whether the body is in HTML or TEXT format. |
| `Attachments` | Attachments | Email | `InArgument<IEnumerable<IResource>>` | No | | The attachments to be sent with the email. |
| `AttachmentInputMode` | Attachment Input Mode | Email | `AttachmentInputMode` | No | `Existing` | How to specify the attachments. |
| `AttachmentList` | Attachment List | Email | `IEnumerable<InArgument<IResource>>` | No | | The list of individual attachments to include. |
| `AttachmentPaths` | Attachment Paths | Email | `InArgument<IEnumerable<string>>` | No | | The full paths of the attachments to be sent with the email. |
| `ArgumentAttachmentPaths` | Attachment Paths (Builder) | Email | `IEnumerable<InArgument<string>>` | No | | The list of file paths of the attachments to be included. |
| `Importance` | Importance | Options | `InArgument<Importance>` | No | `Normal` | Email importance level. |
| `ReplyTo` | Reply To | Options | `InArgument<IEnumerable<string>>` | No | | The email addresses to use when replying. |
| `SaveAsDraft` | Save As Draft | Options | `InArgument<bool>` | No | `True` | Specifies whether the email should be saved as draft. |
| `IsDeliveryReceiptRequested` | Delivery Receipt | Options | `InArgument<bool>` | No | `False` | Specifies whether a delivery receipt was requested. |
| `IsReadReceiptRequested` | Read Receipt | Options | `InArgument<bool>` | No | `False` | Specifies whether a read receipt was requested. |
| `SensitivityLabelId` | Sensitivity Label ID | Options | `InArgument<string>` | No | | The sensitivity label ID for the email. |
| `MailboxArg` | Mailbox | | [`MailboxArgument`](components/MailboxArgument.md) | No | | Specifies the mailbox to use, including optional shared mailbox configuration. See [MailboxArgument](components/MailboxArgument.md) for input modes. |

## Output Properties

| Property | Display Name | Category | Type | Description |
|---|---|---|---|---|
| `Result` | Message ID | Output | `OutArgument<string>` | The message ID of the sent email or draft. |

## Output Model

The `Result` property returns a `string` containing the message ID of the sent email or saved draft.

## Enum Reference

| Enum | Values |
|---|---|
| `BodyInputType` | `HTML`, `TEXT` |
| `AttachmentInputMode` | `Existing`, `Builder`, `FilePaths`, `FilePathsBuilder` |
| `Importance` | `Low`, `Normal`, `High` |

## XAML Example

```xml
<mail:SendMailConnections
    DisplayName="Send Mail"
    To="{x:Null}"
    Subject="Monthly Report"
    Body="&lt;p&gt;Please find the report attached.&lt;/p&gt;"
    InputType="HTML"
    SaveAsDraft="False"
    Importance="Normal" />
```

## Notes

- When `SaveAsDraft` is `True` (default), the email is saved to the Drafts folder instead of being sent.
- Batching is used for performance when there are no attachments and no `Result` binding.
- The `To` property is required; the activity will fail validation without it.
