# GmailMessage

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Models.GmailMessage`

Represents a Gmail email message. Extends `BaseGmailMessage` (which inherits from `System.Net.Mail.MailMessage`) with Gmail-specific properties such as received/sent timestamps and the internet message ID.

**Assembly:** `UiPath.GSuite`
**Inherits:** `UiPath.Shared.Api.Google.Gmail.Models.BaseGmailMessage` -> `System.Net.Mail.MailMessage`
**Implements:** `IMailMetadata`, `ISerializable`

## Properties

### From GmailMessage

| Property | Type | Description |
|----------|------|-------------|
| `ReceivedDateTime` | `DateTime?` | The date and time the message was received. |
| `SentDateTime` | `DateTime?` | The date and time the message was sent. |
| `InternetMessageId` | `string` | The Internet Message ID (RFC 2822 Message-ID header). |
| `WebLink` | `string` | A link to view the message in the Gmail web UI. |

### From BaseGmailMessage

| Property | Type | Description |
|----------|------|-------------|
| `Subject` | `string` | The subject line of the email message. |
| `Body` | `string` | The message body (plain text). |
| `BodyAsHtml` | `string` | The message body as HTML. |
| `FromDisplayName` | `string` | Display name of the sender. |
| `FromAddress` | `string` | Email address of the sender. |
| `SenderDisplayName` | `string` | Display name of the actual sender (may differ from `From` for delegated sending). |
| `SenderAddress` | `string` | Email address of the actual sender. |
| `To` | `MailAddressCollection` | The recipients of this email message. |
| `ToAddressList` | `IEnumerable<string>` | List of recipient email addresses. |
| `CC` | `MailAddressCollection` | The carbon copy (CC) recipients. |
| `CCAddressList` | `IEnumerable<string>` | List of CC email addresses. |
| `Bcc` | `MailAddressCollection` | The blind carbon copy (BCC) recipients. |
| `BccAddressList` | `IEnumerable<string>` | List of BCC email addresses. |
| `ReplyToList` | `MailAddressCollection` | The list of addresses to reply to. |
| `ReplyToAddressList` | `IEnumerable<string>` | List of reply-to email addresses. |
| `MessageId` | `string` | The Gmail message identifier. |
| `ThreadId` | `string` | The ID of the thread the message belongs to. |
| `LabelIds` | `List<string>` | The IDs of labels associated with the message. |
| `FolderName` | `string` | The name of one of the folders the email belongs to. Set when iterating through emails in a certain folder. |
| `StandardAttachmentCount` | `int` | The number of standard (non-inline) attachments. |
| `StandardAttachmentNames` | `IEnumerable<string>` | The filenames of standard attachments. |
| `InlineAttachmentCount` | `int` | The number of inline attachments. |
| `InlineAttachmentNames` | `IEnumerable<string>` | The filenames of inline attachments. |
| `AttachmentCount` | `int` | The total number of attachments (standard + inline). |
| `AttachmentsNamesList` | `IEnumerable<string>` | The filenames of all attachments. |
| `AttachmentsInfoList` | `List<GoogleAttachmentInfoSlim>` | Detailed attachment information objects. |

## Notes

- The `Attachments` property inherited from `MailMessage` is **obsolete** and not populated. Use the "Download Email Attachments" activity to retrieve attachment content.
- Simplified properties (`ToAddressList`, `CCAddressList`, `BccAddressList`, `ReplyToAddressList`) provide convenient `IEnumerable<string>` access to email addresses without working with `MailAddressCollection`.
- This class is serializable for workflow persistence support.

## Used By

Activities that return or accept this type -- see activity docs for details.
