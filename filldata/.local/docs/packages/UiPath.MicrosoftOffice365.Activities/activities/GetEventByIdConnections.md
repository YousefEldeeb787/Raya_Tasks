# Get Event By Id

`UiPath.MicrosoftOffice365.Activities.Calendar.GetEventByIdConnections`

Retrieves a calendar event by its unique identifier.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Calendar |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `EventId` | Event ID | | `InArgument<string>` | Yes | | The unique identifier of the calendar event to retrieve. |
| `CalendarArgument` | Calendar | | [`CalendarArgument`](components/CalendarArgument.md) | No | | Specifies the calendar to search. See [CalendarArgument](components/CalendarArgument.md) for input modes. |

## Output Properties

| Property | Display Name | Category | Type | Description |
|---|---|---|---|---|
| `Event` | Event | Output | [`OutArgument<O365EventItem>`](types/O365EventItem.md) | The calendar event retrieved by the specified ID. |

## Output Model

See [`O365EventItem`](types/O365EventItem.md) for the event object structure.

## XAML Example

```xml
<calendar:GetEventByIdConnections
    DisplayName="Get Event By Id"
    EventId="{x:Null}" />
```

## Notes

- The `EventId` property is required.
