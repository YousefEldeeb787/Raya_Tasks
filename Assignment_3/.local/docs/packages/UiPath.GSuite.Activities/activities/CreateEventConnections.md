# Create Event

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.CreateEventConnections`

Creates a new event in Google Calendar.

**Package:** `UiPath.GSuite.Activities`
**Category:** Calendar
**Connector:** `uipath-google-gmail`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `CalendarArgument` | Calendar | `Property` | [`CalendarArgument`](components/CalendarArgument.md) | No | | The target calendar. See [CalendarArgument](components/CalendarArgument.md) for input modes. |
| `Title` | Title | `InArgument` | `string` | Yes | | The name of the event. |
| `StartDateTime` | Start Date Time | `InArgument` | `DateTimeOffset` | Yes | | The date and time when the event starts. |
| `EndDateTime` | End Date Time | `InArgument` | `DateTimeOffset` | Yes | | The date and time when the event ends. |
| `Timezone` | Timezone | `InArgument` | `string` | Yes | | The timezone for the event's start and end time. |
| `Recurrence` | Recurrence | `InArgument` | `string` | No | | The recurrence rule (RFC 5545 RRULE format). |
| `AllDayEvent` | All Day Event | `InArgument` | `bool` | No | `false` | Indicates if the event takes place all day. Supersedes start/end times. |
| `RequiredAttendees` | Required Attendees | `InArgument` | `IEnumerable<string>` | No | | The list of required attendee email addresses. |
| `OptionalAttendees` | Optional Attendees | `InArgument` | `IEnumerable<string>` | No | | The list of optional attendee email addresses. |
| `ResourceAttendees` | Resource Attendees | `InArgument` | `IEnumerable<string>` | No | | The list of resource attendee email addresses (e.g. meeting rooms). |
| `Location` | Location | `InArgument` | `string` | No | | The location of the event. |
| `Description` | Description | `InArgument` | `string` | No | | The description of the event. |
| `ShowAs` | Show As | `InArgument` | [`EventTransparency`](#enum-reference) | No | `Opaque` | The event status displayed in the calendar (Busy/Available). |
| `Visibility` | Visibility | `InArgument` | [`EventVisibility`](#enum-reference) | No | `DEFAULT` | The visibility label applied on the event. |
| `Status` | Status | `InArgument` | [`EventStatus`](#enum-reference) | No | `CONFIRMED` | The confirmation status of the event. |
| `PreferredReturnTimezone` | Preferred Return Timezone | `InArgument` | `string` | No | | Timezone for the returned event. Defaults to the event timezone. |
| `SendNotification` | Send Notification | `InArgument` | [`SendUpdates`](#enum-reference) | No | `ALL` | Whether to send update notifications to attendees. |
| `GuestCanModifyEvent` | Guest Can Modify Event | `InArgument` | `bool` | No | `false` | Whether guests can modify the event. |
| `GuestCanInviteOthers` | Guest Can Invite Others | `InArgument` | `bool` | No | `true` | Whether guests can invite others. |
| `GuestCanSeeAttendeesList` | Guest Can See Attendees List | `InArgument` | `bool` | No | `true` | Whether guests can see the attendees list. |
| `AddConferenceData` | Add Conference Data | `InArgument` | `bool` | No | `false` | Whether to add conference (Google Meet) data to the event. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | [`GSuiteEventItem`](types/GSuiteEventItem.md) | The created event. |

## Output Model

Returns a [`GSuiteEventItem`](types/GSuiteEventItem.md) with event ID, summary, start/end times, attendees, and other details.

## Enum Reference

| Enum | Values |
|------|--------|
| [`EventTransparency`] | `Opaque`, `Transparent` |
| [`EventVisibility`] | `DEFAULT`, `PUBLIC`, `PRIVATE`, `CONFIDENTIAL` |
| [`EventStatus`] | `CONFIRMED`, `TENTATIVE`, `CANCELLED` |
| [`SendUpdates`] | `ALL`, `EXTERNAL_ONLY`, `NONE` |

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- `Title`, `StartDateTime`, `EndDateTime`, and `Timezone` are required.
- Governance email block-list validation is applied to attendees.
