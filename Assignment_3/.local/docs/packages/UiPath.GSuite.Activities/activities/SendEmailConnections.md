# Send Email

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.SendEmailConnections`

Sends an email using Gmail. Supports HTML or plain text body, file attachments, and saving as draft.

**Package:** `UiPath.GSuite.Activities`
**Category:** Gmail
**Connector:** `uipath-google-gmail`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `To` | To | `InArgument` | `IEnumerable<string>` | No | | The primary recipient email addresses. At least one of To, Cc, or Bcc is required. |
| `Cc` | CC | `InArgument` | `IEnumerable<string>` | No | | The CC (carbon copy) recipient email addresses. |
| `Bcc` | BCC | `InArgument` | `IEnumerable<string>` | No | | The BCC (blind carbon copy) recipient email addresses. |
| `Subject` | Subject | `InArgument` | `string` | No | | The subject line of the email. |
| `Body` | Body | `InArgument` | `string` | No | | The email body content in HTML format. Used when `InputType` is `HTML`. |
| `TextBody` | Text Body | `InArgument` | `string` | No | | The email body content in plain text format. Used when `InputType` is `TEXT`. |
| `InputType` | Input Type | `Property` | [`BodyInputType`](#enum-reference) | No | `HTML` | Specifies whether the body is in HTML or plain text format. |
| `Importance` | Importance | `InArgument` | [`MailImportance`](#enum-reference) | No | `Normal` | The importance level of the email. |
| `ReplyTo` | Reply To | `InArgument` | `IEnumerable<string>` | No | | The email addresses to use when replying. |
| `SaveAsDraft` | Save As Draft | `InArgument` | `bool` | No | `true` | When true, saves the email as a draft instead of sending it immediately. |
| `AttachmentInputMode` | Attachment Input Mode | `Property` | [`AttachmentInputMode`](#enum-reference) | No | `UseExisting` | Specifies how attachments are provided. |
| `Attachments` | Attachments | `InArgument` | `IEnumerable<IResource>` | No | | The attachments collection. Used when `AttachmentInputMode` is `UseExisting`. |
| `ArgumentAttachments` | Argument Attachments | `Property` | `IEnumerable<InArgument<IResource>>` | No | | List of individual attachment resources. Used when `AttachmentInputMode` is `Builder`. |
| `AttachmentPaths` | Attachment Paths | `InArgument` | `IEnumerable<string>` | No | | File paths of attachments. Used when `AttachmentInputMode` is `FilePaths`. |
| `ArgumentAttachmentPaths` | Argument Attachment Paths | `Property` | `IEnumerable<InArgument<string>>` | No | | List of individual file path arguments. Used when `AttachmentInputMode` is `FilePathsBuilder`. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google connection to use. |

## Enum Reference

| Enum | Values |
|------|--------|
| [`BodyInputType`] | `HTML`, `TEXT` |
| [`MailImportance`] | `Low`, `Normal`, `High` |
| [`AttachmentInputMode`] | `UseExisting`, `Builder`, `UseSingle`, `FilePaths`, `FilePathsBuilder` |

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- At least one of `To`, `Cc`, or `Bcc` must be provided.
- When `SaveAsDraft` is `false`, governance email block-list validation is applied.
- The `InputType` property controls which body property is used: `Body` for HTML, `TextBody` for TEXT.
- The `SaveAsDraft` default is `true`; set it to `false` to actually send the email.
