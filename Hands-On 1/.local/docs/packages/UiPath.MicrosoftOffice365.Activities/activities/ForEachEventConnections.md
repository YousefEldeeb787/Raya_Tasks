# For Each Event

`UiPath.MicrosoftOffice365.Activities.Calendar.ForEachEventConnections`

Iterates over a collection of calendar events within a date range. Supports filtering via condition builder or OData query.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Calendar |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `CalendarArgument` | Calendar | | [`CalendarArgument`](components/CalendarArgument.md) | No | | Specifies the calendar to iterate events from. See [CalendarArgument](components/CalendarArgument.md) for input modes. |
| `StartDate` | Start Date | Input | `InArgument<DateTimeOffset>` | Yes | | The date and time as of when to search for events. |
| `EndDate` | End Date | Input | `InArgument<DateTimeOffset>` | Yes | | The date and time until which to search for events. |
| `MaxResults` | Max Results | Options | `InArgument<int>` | No | `50` | The maximum number of events to retrieve and iterate over. |
| `PreferredReturnTimezone` | Preferred Return Timezone | Options | `InArgument<string>` | No | `UTC` | The timezone used to display event start and end times. |
| `Filter` | Filter | | [`EventFilterCollection`](filtering/EventFilterCollection.md) | | | Condition-based filter. See [EventFilterCollection](filtering/EventFilterCollection.md). |

## Output Properties

| Property | Display Name | Category | Type | Description |
|---|---|---|---|---|
| `Length` | Length | Output | `OutArgument<int>` | The number of events processed. |

## Output Model

Each iteration provides an [`O365EventItem`](types/O365EventItem.md) as `CurrentEvent` and an `int` as `CurrentEventIndex`.

## XAML Example

```xml
<calendar:ForEachEventConnections
    DisplayName="For Each Event"
    StartDate="{x:Null}"
    EndDate="{x:Null}"
    MaxResults="50" />
```

## Notes

- `StartDate` and `EndDate` are required.
- The `Body` property contains a sequence of activities executed for each event in the iteration.
- Current item variable name defaults to `CurrentEvent` and current index to `CurrentEventIndex`.
