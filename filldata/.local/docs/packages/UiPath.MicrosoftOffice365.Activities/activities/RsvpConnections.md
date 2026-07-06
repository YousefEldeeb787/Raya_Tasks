# RSVP

`UiPath.MicrosoftOffice365.Activities.Calendar.RsvpConnections`

Responds to a calendar event invitation (accept, tentatively accept, or decline).

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Calendar |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `Event` | Event | Input | [`InArgument<O365EventItem>`](types/O365EventItem.md) | Yes | | The calendar event to respond to. |
| `Response` | Response | Input | `InArgument<EventResponseType>` | No | `Accept` | The RSVP response type. |
| `ApplyOnSeries` | Apply On Series | Input | `InArgument<bool>` | No | `False` | Whether to apply the response on the entire event series. |
| `EmailOrganizer` | Email Organizer | Input | `InArgument<bool>` | No | `False` | Whether to send the RSVP response as an email to the organizer. |
| `Comment` | Comment | Input | `InArgument<string>` | No | | An optional comment included in the RSVP response. |
| `NewStartTime` | New Start Time | Event Scheduling | `InArgument<DateTimeOffset>` | No | | A proposed new start time. |
| `NewEndTime` | New End Time | Event Scheduling | `InArgument<DateTimeOffset>` | No | | A proposed new end time. |
| `NewTimezone` | New Timezone | Event Scheduling | `InArgument<string>` | No | | The timezone for the proposed new times. |

## Output Model

This activity does not return an output object.

## Enum Reference

| Enum | Values |
|---|---|
| `EventResponseType` | `Accept`, `TentativelyAccept`, `Decline` |

## XAML Example

```xml
<calendar:RsvpConnections
    DisplayName="RSVP"
    Event="{x:Null}"
    Response="Accept" />
```

## Notes

- The `Event` property is required.
- The `Comment` property is ignored if `EmailOrganizer` is `False`.
