# New Event Created

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Activities.Calendar.Triggers.NewEventCreated`

Trigger that fires when a new event is created in a specified Google Calendar. Used as a trigger in Orchestrator-managed processes.

**Package:** `UiPath.GSuite.Activities`
**Category:** Calendar
**Connector:** `uipath-google-gmail`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Calendar` | Calendar | `Property` | [`TriggerCalendarArgument`](components/TriggerCalendarArgument.md) | No | | The Google Calendar to monitor for newly created events. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google connection to use. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `Filter` | Filter | [`TriggerEventFilterCollection`](filtering/TriggerEventFilterCollection.md) | | Condition-based filter for matching created events (Title, Organizer, Attendee, Location, etc.). |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | [`GSuiteEventItem`](types/GSuiteEventItem.md) | The created event that activated the trigger. |

## Output Model

Returns a [`GSuiteEventItem`](types/GSuiteEventItem.md) with full event details.

## Notes

- This is a trigger activity designed for use with Orchestrator trigger-based processes.
- In debug mode, retrieves a sample event matching the criteria.
- Only supported with Connection Service authentication.
