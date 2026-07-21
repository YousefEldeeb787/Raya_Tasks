# Modify Event

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.ModifyEventConnections`

Modifies an existing event in Google Calendar. Supports updating title, times, attendees, location, description, and other properties.

**Package:** `UiPath.GSuite.Activities`
**Category:** Calendar
**Connector:** `uipath-google-gmail`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Event` | Event | `InArgument` | [`GSuiteEventItem`](types/GSuiteEventItem.md) | Yes | | The event to modify. |
| `Title` | Title | `InArgument` | `string` | No | | The new name of the event. |
| `StartDateTime` | Start Date Time | `InArgument` | `DateTimeOffset` | No | | The new start date and time. |
| `EndDateTime` | End Date Time | `InArgument` | `DateTimeOffset` | No | | The new end date and time. |
| `Timezone` | Timezone | `InArgument` | `string` | No | | The timezone for the event's start and end time. |
| `AllDayEvent` | All Day Event | `InArgument` | `bool` | No | | Whether the event takes place all day. |
| `Recurrence` | Recurrence | `InArgument` | `string` | No | | The recurrence rule (RFC 5545 RRULE format). |
| `ChangeRequiredAttendees` | Change Required Attendees | `Property` | [`EventPropertyChange`](#enum-reference) | No | `NoChange` | How to modify the required attendees list. |
| `OverwriteRequiredAttendees` | Overwrite Required Attendees | `InArgument` | `IEnumerable<string>` | No | | Replacement list for required attendees (Overwrite mode). |
| `AddRequiredAttendees` | Add Required Attendees | `InArgument` | `IEnumerable<string>` | No | | Attendees to add (AddRemove mode). |
| `RemoveRequiredAttendees` | Remove Required Attendees | `InArgument` | `IEnumerable<string>` | No | | Attendees to remove (AddRemove mode). |
| `ChangeOptionalAttendees` | Change Optional Attendees | `Property` | [`EventPropertyChange`](#enum-reference) | No | `NoChange` | How to modify the optional attendees list. |
| `OverwriteOptionalAttendees` | Overwrite Optional Attendees | `InArgument` | `IEnumerable<string>` | No | | Replacement list for optional attendees. |
| `AddOptionalAttendees` | Add Optional Attendees | `InArgument` | `IEnumerable<string>` | No | | Optional attendees to add. |
| `RemoveOptionalAttendees` | Remove Optional Attendees | `InArgument` | `IEnumerable<string>` | No | | Optional attendees to remove. |
| `ChangeResourceAttendees` | Change Resource Attendees | `Property` | [`EventPropertyChange`](#enum-reference) | No | `NoChange` | How to modify the resource attendees list. |
| `OverwriteResourceAttendees` | Overwrite Resource Attendees | `InArgument` | `IEnumerable<string>` | No | | Replacement list for resource attendees. |
| `AddResourceAttendees` | Add Resource Attendees | `InArgument` | `IEnumerable<string>` | No | | Resource attendees to add. |
| `RemoveResourceAttendees` | Remove Resource Attendees | `InArgument` | `IEnumerable<string>` | No | | Resource attendees to remove. |
| `Location` | Location | `InArgument` | `string` | No | | The event location. |
| `Description` | Description | `InArgument` | `string` | No | | The event description. |
| `ShowAs` | Show As | `InArgument` | [`EventTransparency`](#enum-reference)`?` | No | | The event status displayed in the calendar. |
| `Visibility` | Visibility | `InArgument` | [`EventVisibility`](#enum-reference)`?` | No | | The visibility label applied on the event. |
| `Status` | Status | `InArgument` | [`EventStatus`](#enum-reference)`?` | No | | The confirmation status of the event. |
| `PreferredReturnTimezone` | Preferred Return Timezone | `InArgument` | `string` | No | `UTC` | Timezone for the returned event. |
| `SendNotification` | Send Notification | `InArgument` | [`SendUpdates`](#enum-reference) | No | `ALL` | Whether to send update notifications. |
| `GuestCanModifyEvent` | Guest Can Modify Event | `InArgument` | `bool` | No | | Whether guests can modify the event. |
| `GuestCanInviteOthers` | Guest Can Invite Others | `InArgument` | `bool` | No | | Whether guests can invite others. |
| `GuestCanSeeAttendeesList` | Guest Can See Attendees List | `InArgument` | `bool` | No | | Whether guests can see the attendees list. |
| `AddConferenceData` | Add Conference Data | `InArgument` | `bool` | No | | Whether to add conference data. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | [`GSuiteEventItem`](types/GSuiteEventItem.md) | The modified event. |

## Output Model

Returns a [`GSuiteEventItem`](types/GSuiteEventItem.md) with the updated event details.

## Enum Reference

| Enum | Values |
|------|--------|
| [`EventPropertyChange`] | `NoChange`, `Overwrite`, `AddRemove` |
| [`EventTransparency`] | `Opaque`, `Transparent` |
| [`EventVisibility`] | `DEFAULT`, `PUBLIC`, `PRIVATE`, `CONFIDENTIAL` |
| [`EventStatus`] | `CONFIRMED`, `TENTATIVE`, `CANCELLED` |
| [`SendUpdates`] | `ALL`, `EXTERNAL_ONLY`, `NONE` |

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- The `Event` property is required and must be a [`GSuiteEventItem`](types/GSuiteEventItem.md) from a previous calendar activity.
- Only properties with values set will be updated; unset properties retain their original values.
- Governance email block-list validation is applied to attendees.
