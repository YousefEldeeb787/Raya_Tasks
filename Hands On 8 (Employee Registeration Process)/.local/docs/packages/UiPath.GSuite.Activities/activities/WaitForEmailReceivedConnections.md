# Wait For Email Received

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.WaitForEmailReceivedConnections`

Suspends workflow execution until a new email matching the specified criteria is received in Gmail, then resumes with the received email.

**Package:** `UiPath.GSuite.Activities`
**Category:** Gmail
**Connector:** `uipath-google-gmail`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Folder` | Folder | `Property` | [`FolderArgument`](components/FolderArgument.md) | No | | The Gmail label/folder to monitor. See [FolderArgument](components/FolderArgument.md) for input modes. |
| `WithAttachmentsOnly` | With Attachments Only | `Property` | `bool` | No | `false` | Only trigger for emails with attachments. |
| `IncludeAttachments` | Include Attachments | `InArgument` | `bool` | No | `false` | Whether the returned email should include attachment data. |
| `MarkAsRead` | Mark As Read | `InArgument` | `bool` | No | `false` | Mark the received email as read after retrieval. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google connection to use. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `Filter` | Filter | [`TriggerMailFilterWithVariablesCollection`](filtering/TriggerMailFilterWithVariablesCollection.md) | | Condition-based filter for matching incoming emails. |

### Output

The activity returns a [`GmailMessage`](types/GmailMessage.md) as its `Result` property.

## Output Model

Returns a [`GmailMessage`](types/GmailMessage.md) with the received email details.

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- This is a persistence (long-running) activity. It suspends workflow execution and resumes when a matching email arrives.
- Only supported with Connection Service authentication.
- In debug mode, retrieves a sample email matching the criteria instead of waiting.
