# Find Meeting Times

`UiPath.MicrosoftOffice365.Activities.Calendar.FindMeetingTimesConnections`

Finds available meeting time suggestions for a set of attendees within a time window.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Calendar |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `Attendees` | Attendees | | `InArgument<IEnumerable<string>>` | No | | The email addresses of the attendees to find meeting times for. |
| `IntervalStart` | Interval Start | | `InArgument<DateTimeOffset>` | No | | The start of the time window in which to search. |
| `IntervalEnd` | Interval End | | `InArgument<DateTimeOffset>` | No | | The end of the time window in which to search. |
| `MeetingDuration` | Meeting Duration | | `InArgument<TimeSpan>` | No | `00:30:00` | The desired duration of the meeting. |
| `Timezone` | Timezone | | `InArgument<string>` | No | | The timezone used to interpret the search interval and returned suggestions. |
| `OrganizerRequired` | Organizer Required | | `InArgument<bool>` | No | `True` | Indicates whether the organizer must be available. |
| `Constraint` | Constraint | | `InArgument<MeetingTimeConstraint>` | No | `Work` | The time constraint applied when searching for meeting times. |
| `Locations` | Locations | | `InArgument<IEnumerable<string>>` | No | | The preferred locations to consider. |

## Output Properties

| Property | Display Name | Category | Type | Description |
|---|---|---|---|---|
| `EmptySuggestionsReason` | Empty Suggestions Reason | Output | `OutArgument<string>` | The reason why no meeting time suggestions were returned, if applicable. |

## Output Model

The activity returns an `IEnumerable<MeetingTimeSuggestion>` containing the suggested meeting times.

## Enum Reference

| Enum | Values |
|---|---|
| `MeetingTimeConstraint` | `Work`, `Any` |

## XAML Example

```xml
<calendar:FindMeetingTimesConnections
    DisplayName="Find Meeting Times"
    MeetingDuration="00:30:00" />
```

## Notes

- If `IntervalStart` and `IntervalEnd` are not specified, defaults to the current time to midnight.
- If `Timezone` is not specified, the user's account timezone is used.
