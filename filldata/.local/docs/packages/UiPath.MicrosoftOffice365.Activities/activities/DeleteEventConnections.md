# Delete Event

`UiPath.MicrosoftOffice365.Activities.Calendar.DeleteEventConnections`

Deletes or declines a calendar event.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Calendar |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `Event` | Event | Input | [`InArgument<O365EventItem>`](types/O365EventItem.md) | Yes | | The calendar event to delete or decline. |
| `DeleteOption` | Delete Option | Input | `InArgument<DeleteOptionType>` | No | `SingleEvent` | The option for event deletion. |
| `Comment` | Comment | Input | `InArgument<string>` | No | | The message sent to the attendees/organizer of the deleted event. |

## Output Model

This activity does not return an output object.

## Enum Reference

| Enum | Values |
|---|---|
| `DeleteOptionType` | `SingleEvent`, `ThisAndFollowingEvents`, `AllEvents` |

## XAML Example

```xml
<calendar:DeleteEventConnections
    DisplayName="Delete Event"
    Event="{x:Null}"
    DeleteOption="SingleEvent" />
```

## Notes

- The `Event` property is required.
- Use `DeleteOption` to control whether to delete a single event, this and following events, or all events in a series.
