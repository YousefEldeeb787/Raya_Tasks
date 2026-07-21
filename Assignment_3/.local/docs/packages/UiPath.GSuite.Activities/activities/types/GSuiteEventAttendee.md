# GSuiteEventAttendee

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Calendar.Models.GSuiteEventAttendee`

Represents an attendee of a Google Calendar event, including their response status and role.

**Assembly:** `UiPath.GSuite`

## Properties

| Property | Type | Description |
|----------|------|-------------|
| `DisplayName` | `string` | Display name of the attendee. |
| `Email` | `string` | Email address of the attendee. |
| `Comment` | `string` | The attendee's response comment. |
| `Optional` | `bool?` | Whether the attendee is optional. |
| `Resource` | `bool?` | Whether the attendee is a resource (e.g., a conference room). |
| `Organizer` | `bool?` | Whether the attendee is the organizer. |
| `ResponseStatus` | `GSuiteEventAttendeeStatus` | The attendee's response status. |

## GSuiteEventAttendeeStatus Enum

| Value | Description |
|-------|-------------|
| `DeedsAction` | The attendee has not responded (needs action). |
| `Declined` | The attendee declined the invitation. |
| `Tentative` | The attendee tentatively accepted. |
| `Accepted` | The attendee accepted the invitation. |
| `Unknown` | The response status is unknown. |

## Notes

- This type appears as elements in the `Attendees` collection of [GSuiteEventItem](GSuiteEventItem.md).

## Used By

Activities that return or accept this type -- see activity docs for details.
