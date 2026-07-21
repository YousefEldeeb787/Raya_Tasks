# GSuiteEventItem

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Calendar.Models.GSuiteEventItem`

Represents a Google Calendar event with all its metadata including timing, attendees, recurrence, and organizer information.

**Assembly:** `UiPath.GSuite`

## Properties

| Property | Type | Description |
|----------|------|-------------|
| `Summary` | `string` | Event title/subject. |
| `Description` | `string` | Event description. |
| `Attendees` | `IEnumerable<`[`GSuiteEventAttendee`](GSuiteEventAttendee.md)`>` | Attendee list. |
| `AttendeesEmailList` | `IEnumerable<string>` | List of attendee email addresses (derived from `Attendees`). |
| `Start` | [`DateTimeTimeZone`](DateTimeTimeZone.md) | Start time with timezone information. |
| `StartDateTime` | `DateTime` | Start date (convenience accessor; returns `DateTime.MinValue` if `Start` is null). |
| `End` | [`DateTimeTimeZone`](DateTimeTimeZone.md) | End time with timezone information. |
| `EndDateTime` | `DateTime` | End date (convenience accessor; returns `DateTime.MinValue` if `End` is null). |
| `Organizer` | [`GSuiteOrganizer`](GSuiteOrganizer.md) | The organizer of the event. |
| `OrganizerDisplayName` | `string` | Display name of the organizer (derived from `Organizer`). |
| `OrganizerEmail` | `string` | Email address of the organizer (derived from `Organizer`). |
| `Type` | `string` | The type of the event. |
| `Created` | `DateTimeOffset?` | Created datetime. |
| `Recurrence` | `IList<string>` | Recurrence rules (RFC 5545 RRULE format). |
| `Updated` | `DateTimeOffset?` | Last modified datetime. |
| `Location` | `string` | Event location. |
| `Visibility` | `string` | Event visibility (e.g., "default", "public", "private"). |
| `Transparency` | `string` | Show-as status (e.g., "opaque" for busy, "transparent" for free). |
| `WebLink` | `string` | An absolute link to this event in the Google Calendar Web UI. |
| `Id` | `string` | The unique identifier of the event. |
| `CalendarId` | `string` | The calendar ID that this event belongs to. |
| `PreferredReturnTimezone` | `string` | The preferred timezone for returned datetime values. |
| `ICalUId` | `string` | ICal event ID. |

## Notes

- `StartDateTime` and `EndDateTime` are simplified accessors that discard timezone information. Use `Start` and `End` for timezone-aware operations.
- `AttendeesEmailList` and `OrganizerEmail` are convenience properties for simplified property access.

## Used By

Activities that return or accept this type -- see activity docs for details.
