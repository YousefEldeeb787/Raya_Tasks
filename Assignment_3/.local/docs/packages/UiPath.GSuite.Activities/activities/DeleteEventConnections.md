# Delete Event

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.DeleteEventConnections`

Deletes an event from Google Calendar.

**Package:** `UiPath.GSuite.Activities`
**Category:** Calendar
**Connector:** `uipath-google-gmail`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Event` | Event | `InArgument` | [`GSuiteEventItem`](types/GSuiteEventItem.md) | Yes | | The event to delete. |
| `DeleteType` | Delete Type | `InArgument` | [`DeleteEventType`](#enum-reference) | No | `SingleEvent` | The scope of deletion for recurring events. |
| `SendNotification` | Send Notification | `InArgument` | [`SendUpdates`](#enum-reference) | No | `ALL` | Whether to send cancellation notifications. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google connection to use. |

## Enum Reference

| Enum | Values |
|------|--------|
| [`DeleteEventType`] | `SingleEvent`, `FutureOnly`, `PastAndFuture` |
| [`SendUpdates`] | `ALL`, `EXTERNAL_ONLY`, `NONE` |

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- The `Event` property is required.
