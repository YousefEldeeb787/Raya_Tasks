# Download Attachments

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.DownloadAttachmentsConnections`

Downloads email attachments from a Gmail message to the local filesystem.

**Package:** `UiPath.GSuite.Activities`
**Category:** Gmail
**Connector:** `uipath-google-gmail`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Email` | Email | `InArgument` | [`GmailMessage`](types/GmailMessage.md) | Yes | | The email to get the attachments from. |
| `ExcludeInlineAttachments` | Exclude Inline Attachments | `InArgument` | `bool` | No | `false` | Indicates whether to exclude inline attachments (embedded in the email body). |
| `FilterByFileNames` | Filter By File Names | `InArgument` | `string` | No | | Download only attachments whose name matches the specified pattern. Multiple patterns separated by `\|`. |
| `DestinationPath` | Destination Path | `InArgument` | `string` | No | | The local path where the downloaded attachments are saved. |
| `ConflictResolution` | Conflict Resolution | `InArgument` | [`ConflictBehavior`](#enum-reference) | No | `Fail` | Conflict resolution behavior for files with the same name. |
| `SearchMode` | Search Mode | `Property` | [`SearchSelectionMode`](#enum-reference) | No | `UseSimple` | Toggle between simple filename filter and advanced filter conditions. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google connection to use. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `Filter` | Filter | [`FilenameFilterCollection`](filtering/FilenameFilterCollection.md) | | Advanced filename filter conditions. Used when `SearchMode` is `UseAdvanced`. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `NewResult` | Result | `OutArgument` | [`GmailAttachmentLocalItem`](types/GmailAttachmentLocalItem.md)`[]` | The downloaded attachment files. |

## Enum Reference

| Enum | Values |
|------|--------|
| [`ConflictBehavior`] | `Replace`, `Fail`, `Rename` |
| [`SearchSelectionMode`] | `UseSimple`, `UseAdvanced` |

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- The `Email` property is required.
- Throws an error if the destination path does not exist.
