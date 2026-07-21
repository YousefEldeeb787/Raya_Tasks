# Get Calendars

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.GetCalendarsConnections`

Retrieves the list of Google Calendars accessible to the authenticated user.

**Package:** `UiPath.GSuite.Activities`
**Category:** Calendar
**Connector:** `uipath-google-gmail`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Calendars` | Calendars | `OutArgument` | `List<`[`GSuiteCalendarItem`](types/GSuiteCalendarItem.md)`>` | The list of calendars. |
| `DefaultCalendar` | Default Calendar | `OutArgument` | [`GSuiteCalendarItem`](types/GSuiteCalendarItem.md) | The user's primary calendar. |

## Output Model

Returns a `List<`[`GSuiteCalendarItem`](types/GSuiteCalendarItem.md)`>` and the primary [`GSuiteCalendarItem`](types/GSuiteCalendarItem.md).

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- No input properties are required.
