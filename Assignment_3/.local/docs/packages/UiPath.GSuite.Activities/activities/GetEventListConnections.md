# Get Event List

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.GetEventListConnections`

Retrieves a list of events from a Google Calendar within a specified date range.

**Package:** `UiPath.GSuite.Activities`
**Category:** Calendar
**Connector:** `uipath-google-gmail`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `CalendarArgument` | Calendar | `Property` | [`CalendarArgument`](components/CalendarArgument.md) | No | | The target calendar. See [CalendarArgument](components/CalendarArgument.md) for input modes. |
| `StartDate` | Start Date | `InArgument` | `DateTimeOffset` | Yes | | The start of the date range to search. |
| `EndDate` | End Date | `InArgument` | `DateTimeOffset` | Yes | | The end of the date range to search. |
| `MaxResults` | Max Results | `InArgument` | `int` | No | `50` | The maximum number of events to retrieve. If 0 or negative, all matching events are returned. |
| `PreferredReturnTimezone` | Preferred Return Timezone | `InArgument` | `string` | No | `UTC` | Timezone for the returned events. |
| `SimpleSearch` | Simple Search | `InArgument` | `string` | No | | Text to filter within calendar events. |
| `ReturnRecurring` | Return Recurring | `InArgument` | `bool` | No | `true` | Whether to return individual recurring event instances. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `EventList` | Event List | `OutArgument` | `List<`[`GSuiteEventItem`](types/GSuiteEventItem.md)`>` | The retrieved event list. |

## Output Model

Returns a `List<`[`GSuiteEventItem`](types/GSuiteEventItem.md)`>` with event details.

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- `StartDate` and `EndDate` are required.
