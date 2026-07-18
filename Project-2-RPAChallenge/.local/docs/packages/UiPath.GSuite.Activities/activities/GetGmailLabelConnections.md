# Get Gmail Label

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.GetGmailLabelConnections`

Retrieves the Gmail label with the specified ID, including message/thread counts and visibility settings.

**Package:** `UiPath.GSuite.Activities`
**Category:** Gmail
**Connector:** `uipath-google-gmail`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `LabelId` | Label Id | `InArgument` | `string` | Yes | | The unique identifier of the Gmail label. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google connection to use. |

### Output

This activity extends `GoogleBaseActivity<GmailFullLabelItem>` and returns a [`GmailFullLabelItem`](types/GmailFullLabelItem.md) as its `Result` property.

## Output Model

Returns a [`GmailFullLabelItem`](types/GmailFullLabelItem.md) with label details including name, ID, message count, and visibility settings.

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- The `LabelId` property is required.
