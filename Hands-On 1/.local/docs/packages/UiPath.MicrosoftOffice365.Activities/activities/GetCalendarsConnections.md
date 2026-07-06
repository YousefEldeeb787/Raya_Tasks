# Get Calendars

`UiPath.MicrosoftOffice365.Activities.Calendar.GetCalendarsConnections`

Gets the user's calendars, optionally including group calendars.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Calendar |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `IncludeGroupCalendars` | Include Group Calendars | Input | `bool` | No | `False` | Specifies whether to include group calendars in the returned list. |

## Output Properties

| Property | Display Name | Category | Type | Description |
|---|---|---|---|---|
| `Calendars` | Calendars | Output | `OutArgument<List<O365CalendarItem>>` | The list of calendars retrieved for the user. |
| `DefaultCalendar` | Default Calendar | Output | `OutArgument<O365CalendarItem>` | The user's default calendar, if one exists. |

## Output Model

Each `O365CalendarItem` represents a calendar with its properties.

## XAML Example

```xml
<calendar:GetCalendarsConnections
    DisplayName="Get Calendars"
    IncludeGroupCalendars="False" />
```

## Notes

- Returns both personal and group calendars when `IncludeGroupCalendars` is `True`.
