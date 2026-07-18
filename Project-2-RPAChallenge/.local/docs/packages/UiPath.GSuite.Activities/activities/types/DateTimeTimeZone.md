# DateTimeTimeZone

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Calendar.Models.DateTimeTimeZone`

Represents a date/time value paired with its timezone. Used by [GSuiteEventItem](GSuiteEventItem.md) to express event start and end times with timezone context.

**Assembly:** `UiPath.GSuite`

## Properties

| Property | Type | Description |
|----------|------|-------------|
| `DateTime` | `DateTime` | The date and time value. |
| `TimeZone` | `string` | The timezone identifier (e.g., "America/New_York", "Europe/London"). |

## Notes

- This type is used as the `Start` and `End` properties of [GSuiteEventItem](GSuiteEventItem.md).
- The `TimeZone` string follows the IANA Time Zone Database format.

## Used By

Activities that return or accept this type -- see activity docs for details.
