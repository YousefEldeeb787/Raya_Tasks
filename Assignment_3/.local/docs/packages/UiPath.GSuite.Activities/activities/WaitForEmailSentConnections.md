# Wait For Email Sent

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.WaitForEmailSentConnections`

Suspends workflow execution until an email matching the specified criteria is sent from Gmail, then resumes with the sent email.

**Package:** `UiPath.GSuite.Activities`
**Category:** Gmail
**Connector:** `uipath-google-gmail`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `WithAttachmentsOnly` | With Attachments Only | `Property` | `bool` | No | `false` | Only trigger for sent emails with attachments. |
| `IncludeAttachments` | Include Attachments | `InArgument` | `bool` | No | `false` | Whether the returned email should include attachment data. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google connection to use. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `Filter` | Filter | [`TriggerMailFilterWithVariablesCollection`](filtering/TriggerMailFilterWithVariablesCollection.md) | | Condition-based filter for matching sent emails. |

### Output

The activity returns a [`GmailMessage`](types/GmailMessage.md) as its `Result` property.

## Output Model

Returns a [`GmailMessage`](types/GmailMessage.md) with the sent email details.

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- This is a persistence (long-running) activity. It suspends workflow execution and resumes when a matching sent email is detected.
- Only supported with Connection Service authentication.
- In debug mode, retrieves a sample sent email matching the criteria instead of waiting.
