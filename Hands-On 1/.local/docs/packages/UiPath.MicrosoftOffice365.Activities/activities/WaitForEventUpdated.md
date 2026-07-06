# Wait For Event Updated

`UiPath.MicrosoftOffice365.Activities.WaitForEventUpdated`

Suspends workflow execution and waits until a calendar event is updated. This is a persistent (long-running) activity.

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
<activities:WaitForEventUpdated
    DisplayName="Wait For Event Updated" />
```

## Notes

- This is a persistent activity that suspends workflow execution until the trigger fires.
- Supports filtering via the `Filter` property (condition builder with variables).
- In debug mode, retrieves the most recent matching updated event instead of waiting.
