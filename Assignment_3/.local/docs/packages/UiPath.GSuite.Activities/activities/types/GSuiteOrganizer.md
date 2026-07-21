# GSuiteOrganizer

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Calendar.Models.GSuiteOrganizer`

Represents the organizer of a Google Calendar event.

**Assembly:** `UiPath.GSuite`

## Properties

| Property | Type | Description |
|----------|------|-------------|
| `DisplayName` | `string` | Display name of the organizer. |
| `Email` | `string` | Email address of the organizer. |
| `Id` | `string` | The organizer's Profile ID, if available. |
| `Self` | `bool?` | Whether the organizer corresponds to the calendar on which this copy of the event appears. The default is False. |

## Notes

- This type is used as the `Organizer` property of [GSuiteEventItem](GSuiteEventItem.md).
- The `Self` property is useful for determining if the event was organized by the calendar owner.

## Used By

Activities that return or accept this type -- see activity docs for details.
