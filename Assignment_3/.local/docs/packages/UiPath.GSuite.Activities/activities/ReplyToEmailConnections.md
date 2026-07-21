# Reply To Email

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.ReplyToEmailConnections`

Replies to an existing Gmail email. Supports adding new recipients, attachments, and saving as draft.

**Package:** `UiPath.GSuite.Activities`
**Category:** Gmail
**Connector:** `uipath-google-gmail`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Email` | Email | `InArgument` | [`GmailMessage`](types/GmailMessage.md) | Yes | | The email to reply to. |
| `Body` | Body | `InArgument` | `string` | No | | The HTML body of the reply. |
| `NewSubject` | New Subject | `InArgument` | `string` | No | | The new subject. If left blank, the original subject is used. |
| `To` | To | `InArgument` | `IEnumerable<string>` | No | | Additional primary recipients. |
| `Cc` | CC | `InArgument` | `IEnumerable<string>` | No | | Additional CC recipients. |
| `Bcc` | BCC | `InArgument` | `IEnumerable<string>` | No | | Additional BCC recipients. |
| `Importance` | Importance | `InArgument` | [`MailImportance`](#enum-reference) | No | `Normal` | The importance level of the reply. |
| `SaveAsDraft` | Save As Draft | `InArgument` | `bool` | No | `true` | When true, saves the reply as a draft instead of sending it. |
| `ReplyToAll` | Reply To All | `InArgument` | `bool` | No | `false` | When true, replies to all original recipients. |
| `AttachmentInputMode` | Attachment Input Mode | `Property` | [`AttachmentInputMode`](#enum-reference) | No | `UseExisting` | Specifies how attachments are provided. |
| `Attachments` | Attachments | `InArgument` | `IEnumerable<IResource>` | No | | The attachments to send with the reply. |
| `ArgumentAttachments` | Argument Attachments | `Property` | `IEnumerable<InArgument<IResource>>` | No | | List of individual attachment resources for builder mode. |
| `AttachmentPaths` | Attachment Paths | `InArgument` | `IEnumerable<string>` | No | | File paths of attachments. |
| `ArgumentAttachmentPaths` | Argument Attachment Paths | `Property` | `IEnumerable<InArgument<string>>` | No | | List of individual file path arguments for builder mode. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google connection to use. |

## Enum Reference

| Enum | Values |
|------|--------|
| [`MailImportance`] | `Low`, `Normal`, `High` |
| [`AttachmentInputMode`] | `UseExisting`, `Builder`, `UseSingle`, `FilePaths`, `FilePathsBuilder` |

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- The `Email` property is required and must be a [`GmailMessage`](types/GmailMessage.md) obtained from a previous activity.
- The reply body is always sent as HTML.
- When `SaveAsDraft` is `false`, governance email block-list validation is applied.
