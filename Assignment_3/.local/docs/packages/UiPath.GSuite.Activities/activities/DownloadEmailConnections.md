# Download Email

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.DownloadEmailConnections`

Downloads an email as an .eml file from Gmail to the local filesystem.

**Package:** `UiPath.GSuite.Activities`
**Category:** Gmail
**Connector:** `uipath-google-gmail`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Email` | Email | `InArgument` | [`GmailMessage`](types/GmailMessage.md) | Yes | | The email to download. |
| `DestinationPath` | Destination Path | `InArgument` | `string` | No | | The local path where the downloaded email file is saved. |
| `ConflictResolution` | Conflict Resolution | `InArgument` | [`ConflictBehavior`](#enum-reference) | No | `Fail` | Conflict resolution behavior when a file with the same name already exists. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `NewResult` | Result | `OutArgument` | [`GmailMessageLocalItem`](types/GmailMessageLocalItem.md) | The downloaded email file. |

## Enum Reference

| Enum | Values |
|------|--------|
| [`ConflictBehavior`] | `Replace`, `Fail`, `Rename` |

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- The `Email` property is required.
- If `DestinationPath` is not specified, the file is available as an in-memory resource.
