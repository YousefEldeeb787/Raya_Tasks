# Wait For Event Invitation Received and Resume

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Activities.WaitForEventInvitationReceived`

Suspends workflow execution until a new event invitation is received in Google Calendar matching the specified criteria, then resumes with the event.

**Package:** `UiPath.GSuite.Activities`
**Category:** Calendar
**Connector:** `uipath-google-gmail`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `CalendarArgument` | Calendar | `Property` | [`CalendarArgument`](components/CalendarArgument.md) | No | | The target calendar to monitor. See [CalendarArgument](components/CalendarArgument.md) for input modes. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google connection to use. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `Filter` | Filter | [`EventFilterWithVariablesCollection`](filtering/EventFilterWithVariablesCollection.md) | | Condition-based filter for matching received invitations. |

### Output

The activity returns a [`GSuiteEventItem`](types/GSuiteEventItem.md) as its `Result` property.

## Output Model

Returns a [`GSuiteEventItem`](types/GSuiteEventItem.md) with the invitation event details.

## Notes

- This is a persistence (long-running) activity. It suspends workflow execution and resumes when a matching event invitation is received.
- Only supported with Connection Service authentication.
- In debug mode, retrieves a sample event matching the criteria instead of waiting.
