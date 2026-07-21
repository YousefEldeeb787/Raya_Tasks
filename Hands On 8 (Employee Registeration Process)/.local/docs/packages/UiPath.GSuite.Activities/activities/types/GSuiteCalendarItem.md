# GSuiteCalendarItem

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Calendar.Models.GSuiteCalendarItem`

Represents a Google Calendar. Used to identify which calendar an operation targets.

**Assembly:** `UiPath.GSuite`

## Properties

| Property | Type | Description |
|----------|------|-------------|
| `Id` | `string` | The unique identifier of the calendar. |
| `Summary` | `string` | Title of the calendar. |
| `Description` | `string` | Description of the calendar. |
| `Location` | `string` | Geographic location of the calendar. |
| `TimeZone` | `string` | The time zone of the calendar. |
| `Primary` | `bool?` | True if this is the authenticated user's primary calendar. |

## Notes

- The `Id` is required and set at construction time. A null ID throws `ArgumentNullException`.
- Equality is determined by `Id` -- two [GSuiteCalendarItem](GSuiteCalendarItem.md) instances with the same `Id` are considered equal.

## Used By

Activities that return or accept this type -- see activity docs for details. Also used by [CalendarArgument](../components/CalendarArgument.md) in `UseExisting` mode.
