# New Event Invitation Received (Trigger)

`UiPath.MicrosoftOffice365.Activities.Calendar.Triggers.NewEventInvitationReceived`

Trigger that fires when a new calendar event invitation is received.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Calendar |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `CalendarArgument` | Calendar | | [`CalendarArgument`](components/CalendarArgument.md) | No | | Specifies the calendar to monitor. See [CalendarArgument](components/CalendarArgument.md) for input modes. |

## Output Properties

| Property | Display Name | Category | Type | Description |
|---|---|---|---|---|
| `Result` | Event | Output | [`OutArgument<O365EventItem>`](types/O365EventItem.md) | The event invitation that triggered the activity. |

## Output Model

See [`O365EventItem`](types/O365EventItem.md) for the event object structure.

## XAML Example

```xml
<triggers:NewEventInvitationReceived
    DisplayName="New Event Invitation Received" />
```

## Notes

- This is a trigger activity used to start workflows when an event invitation is received.
- Supports filtering via the `Filter` property (condition builder).
- In debug mode, retrieves the most recent matching event invitation.
