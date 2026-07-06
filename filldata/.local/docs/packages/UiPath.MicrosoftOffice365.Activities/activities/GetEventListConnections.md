# Get Event List

`UiPath.MicrosoftOffice365.Activities.Calendar.GetEventListConnections`

Gets the list of calendar events within a date range. Supports filtering via condition builder or OData query.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Calendar |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `CalendarArgument` | Calendar | | [`CalendarArgument`](components/CalendarArgument.md) | No | | Specifies the calendar to search. See [CalendarArgument](components/CalendarArgument.md) for input modes. |
| `StartDate` | Start Date | Input | `InArgument<DateTimeOffset>` | Yes | | The date and time as of when to search for events. |
| `EndDate` | End Date | Input | `InArgument<DateTimeOffset>` | Yes | | The date and time until which to search for events. |
| `MaxResults` | Max Results | Options | `InArgument<int>` | No | `50` | The maximum number of events to retrieve. |
| `PreferredReturnTimezone` | Preferred Return Timezone | Options | `InArgument<string>` | No | `UTC` | The timezone used to display event start and end times. |
| `Filter` | Filter | | [`EventFilterCollection`](filtering/EventFilterCollection.md) | | | Condition-based filter. See [EventFilterCollection](filtering/EventFilterCollection.md). |

## Output Properties

| Property | Display Name | Category | Type | Description |
|---|---|---|---|---|
| `EventList` | Event List | Output | `OutArgument<List<`[`O365EventItem`](types/O365EventItem.md)`>>` | The processed event list. |

## Output Model

See [`O365EventItem`](types/O365EventItem.md) for the event object structure.

## XAML Example

```xml
<calendar:GetEventListConnections
    DisplayName="Get Event List"
    StartDate="{x:Null}"
    EndDate="{x:Null}"
    MaxResults="50" />
```

## Notes

- `StartDate` and `EndDate` are required.
- Default MaxResults is 50. Set to 0 or a negative value to retrieve all events.
- Supports filtering via condition builder or OData query.
