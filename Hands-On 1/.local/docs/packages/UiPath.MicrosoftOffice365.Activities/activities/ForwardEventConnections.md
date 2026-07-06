# Forward Event

`UiPath.MicrosoftOffice365.Activities.Calendar.ForwardEventConnections`

Forwards a calendar event to new attendees.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Calendar |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `Event` | Event | Input | [`InArgument<O365EventItem>`](types/O365EventItem.md) | Yes | | The event to forward. |
| `Attendees` | Attendees | Input | `InArgument<IEnumerable<string>>` | Yes | | Attendees to forward the event to. |
| `ApplyOnSeries` | Apply On Series | Input | `InArgument<bool>` | No | `False` | Forward single instance or entire series. |
| `Comment` | Comment | Input | `InArgument<string>` | No | | Optional comment when forwarding. |

## Output Model

This activity does not return an output object.

## XAML Example

```xml
<calendar:ForwardEventConnections
    DisplayName="Forward Event"
    Event="{x:Null}"
    Attendees="{x:Null}" />
```

## Notes

- Both `Event` and `Attendees` are required.
