# Apply Email Labels

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.ApplyEmailLabelsConnections`

Applies one or more labels to an email in Gmail.

**Package:** `UiPath.GSuite.Activities`
**Category:** Gmail
**Connector:** `uipath-google-gmail`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Email` | Email | `InArgument` | [`GmailMessage`](types/GmailMessage.md) | Yes | | The email to modify labels on. |
| `LabelSelectionMode` | Label Selection Mode | `Property` | [`LabelInputMode`](#enum-reference) | No | | Labels input mode (multi-select from browser or variable). |
| `SelectedLabels` | Selected Labels | `Property` | `string` | Conditional | | The list of selected labels (serialized). Used with `MultiSelect` mode. |
| `ManualEntrySelectedLabels` | Manual Entry Selected Labels | `InArgument` | `IEnumerable<string>` | Conditional | | The list of label names provided as a variable. Used with `Variable` mode. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google connection to use. |

## Enum Reference

| Enum | Values |
|------|--------|
| [`LabelInputMode`] | `MultiSelect`, `Variable` |

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- The `Email` property is required.
- Either `SelectedLabels` (MultiSelect mode) or `ManualEntrySelectedLabels` (Variable mode) must be provided depending on the `LabelSelectionMode`.
