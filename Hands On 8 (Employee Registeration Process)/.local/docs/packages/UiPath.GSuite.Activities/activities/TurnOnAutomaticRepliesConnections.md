# Turn On Automatic Replies

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.TurnOnAutomaticRepliesConnections`

Activates and configures Gmail vacation/out-of-office automatic replies.

**Package:** `UiPath.GSuite.Activities`
**Category:** Gmail
**Connector:** `uipath-google-gmail`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `StartTime` | Start Time | `InArgument` | `DateTimeOffset` | No | | The date and time when the Out of Office starts. |
| `EndTime` | End Time | `InArgument` | `DateTimeOffset` | No | | The date and time when the Out of Office ends. |
| `MessageBodyHtml` | Message Body HTML | `InArgument` | `string` | No | | The automatic reply message body (HTML). At least one of MessageBodyHtml or MessageSubject is required. |
| `MessageSubject` | Message Subject | `InArgument` | `string` | No | | The automatic reply subject line. At least one of MessageBodyHtml or MessageSubject is required. |
| `SendRepliesOutsideOrganization` | Send Replies Outside Organization | `InArgument` | `bool` | No | `false` | Whether automatic replies can be sent to users outside the organization. |
| `SendRepliesToContactsOnly` | Send Replies To Contacts Only | `InArgument` | `bool` | No | `false` | Whether to send automatic replies only to contacts when replying to users outside the organization. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google connection to use. |

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- At least one of `MessageBodyHtml` or `MessageSubject` must be provided.
