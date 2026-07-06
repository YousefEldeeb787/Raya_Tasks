# Event Replied (Trigger)

`UiPath.MicrosoftOffice365.Activities.Calendar.Triggers.EventReplied`

Trigger that fires when a calendar event receives a reply.

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
| `Result` | Event | Output | [`OutArgument<O365EventItem>`](types/O365EventItem.md) | The event that received a reply. |

## Output Model

See [`O365EventItem`](types/O365EventItem.md) for the event object structure.

## XAML Example

```xml
<triggers:EventReplied
    DisplayName="Event Replied" />
```

## Notes

- This is a trigger activity used to start workflows when an event reply is received.
- Supports filtering via the `Filter` property (condition builder).
- In debug mode, retrieves the most recent matching event reply.
