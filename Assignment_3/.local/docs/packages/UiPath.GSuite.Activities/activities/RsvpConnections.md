# RSVP

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.RsvpConnections`

Responds to a Google Calendar event invitation (Accept, Decline, or Tentative).

**Package:** `UiPath.GSuite.Activities`
**Category:** Calendar
**Connector:** `uipath-google-gmail`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Event` | Event | `InArgument` | [`GSuiteEventItem`](types/GSuiteEventItem.md) | Yes | | The event to respond to. |
| `Response` | Response | `InArgument` | [`EventResponseType`](#enum-reference) | No | `Accepted` | The RSVP response to send. |
| `ApplyOnSeries` | Apply On Series | `InArgument` | `bool` | No | `false` | Whether to apply the response on the entire recurring series. |
| `EmailOrganizer` | Email Organizer | `InArgument` | `bool` | No | `false` | Whether to email the organizer with the response. |
| `Comment` | Comment | `InArgument` | `string` | No | | An optional text message to include with the RSVP response. |
| `AdditionalGuests` | Additional Guests | `InArgument` | `int` | No | | The number of additional guests attending beyond the current user. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google connection to use. |

## Enum Reference

| Enum | Values |
|------|--------|
| [`EventResponseType`] | `Declined`, `Tentative`, `Accepted` |

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- The `Event` property is required.
