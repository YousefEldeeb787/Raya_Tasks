# Wait For Event Created

`UiPath.MicrosoftOffice365.Activities.WaitForEventCreated`

Suspends workflow execution and waits until a new calendar event is created. This is a persistent (long-running) activity.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Calendar |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `CalendarArgument` | Calendar | | [`CalendarArgument`](components/CalendarArgument.md) | No | | Specifies the calendar to monitor. See [CalendarArgument](components/CalendarArgument.md) for input modes. |

## Output Model

The activity returns an [`O365EventItem`](types/O365EventItem.md) as its result.

See [`O365EventItem`](types/O365EventItem.md) for the event object structure.

## XAML Example

```xml
<activities:WaitForEventCreated
    DisplayName="Wait For Event Created" />
```

## Notes

- This is a persistent activity that suspends workflow execution until the trigger fires.
- Supports filtering via the `Filter` property (condition builder with variables).
- In debug mode, retrieves the most recent matching event instead of waiting.
