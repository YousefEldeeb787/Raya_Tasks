# For Each Email

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.ForEachEmailConnections`

Iterates over a collection of emails from a specified Gmail folder/label, executing contained activities for each email.

**Package:** `UiPath.GSuite.Activities`
**Category:** Gmail
**Connector:** `uipath-google-gmail`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Folder` | Folder | `Property` | [`FolderArgument`](components/FolderArgument.md) | No | | The Gmail label/folder. See [FolderArgument](components/FolderArgument.md) for input modes. |
| `CurrentItemVariableName` | Current Item Variable Name | `Property` | `string` | Yes | `CurrentEmail` | Identifier for the current [`GmailMessage`](types/GmailMessage.md) in the iteration. |
| `MaxResults` | Max Results | `InArgument` | `int` | No | `100` | The number of emails to iterate through. |
| `IncludeSubfolders` | Include Subfolders | `InArgument` | `bool` | No | `false` | Specifies whether to expand the search to include all subfolders. |
| `UnreadOnly` | Unread Only | `InArgument` | `bool` | No | `false` | Indicates whether to consider only unread emails. |
| `WithAttachmentsOnly` | With Attachments Only | `InArgument` | `bool` | No | `false` | Indicates whether to consider only emails with attachments. |
| `ImportantOnly` | Important Only | `InArgument` | `bool` | No | `false` | Return only important emails. |
| `StarredOnly` | Starred Only | `InArgument` | `bool` | No | `false` | Return only starred emails. |
| `MarkAsRead` | Mark As Read | `InArgument` | `bool` | No | `false` | Indicates whether to mark the retrieved email as read. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google connection to use. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `FilterSelectionMode` | Filter Selection Mode | [`FilterMode`](#enum-reference) | `ConditionBuilder` | How filters are specified: via condition builder or a raw query string. |
| `QueryFilter` | Query Filter | `InArgument<string>` | | A raw Gmail search query string. Used when `FilterSelectionMode` is `Query`. |
| `Filter` | Filter | [`MailFilterCollection`](filtering/MailFilterCollection.md) | | Condition-based filter. See [MailFilterCollection](filtering/MailFilterCollection.md) for criteria and operators. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Length` | Length | `OutArgument` | `int` | The number of emails processed. |

## Enum Reference

| Enum | Values |
|------|--------|
| [`FilterMode`] | `ConditionBuilder`, `Query` |

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- This is a container activity; the `Body` contains the activities to execute for each [`GmailMessage`](types/GmailMessage.md).
- The current email is available via the `CurrentItemVariableName` delegate argument (default: `CurrentEmail`).
- The current index is available via `CurrentEmailIndex`.
