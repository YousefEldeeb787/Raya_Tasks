# Email Sent

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Activities.Gmail.Triggers.EmailSent`

Trigger that fires when an email is sent from the authenticated Gmail account. Used as a trigger in Orchestrator-managed processes.

**Package:** `UiPath.GSuite.Activities`
**Category:** Gmail
**Connector:** `uipath-google-gmail`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `WithAttachmentsOnly` | With Attachments Only | `Property` | `bool` | No | `false` | Retrieve only sent emails with attachments. |
| `IncludeAttachments` | Include Attachments | `InArgument` | `bool` | No | `false` | Whether the returned email should include attachment data. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google connection to use. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `Filter` | Filter | [`TriggerMailFilterCollection`](filtering/TriggerMailFilterCollection.md) | | Condition-based filter for matching sent emails. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | [`GmailMessage`](types/GmailMessage.md) | The sent email that activated the trigger. |

## Output Model

Returns a [`GmailMessage`](types/GmailMessage.md) with full email details.

## Notes

- This is a trigger activity designed for use with Orchestrator trigger-based processes.
- In debug mode, retrieves a sample sent email matching the criteria.
- Only supported with Connection Service authentication.
