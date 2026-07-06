# CalendarArgument

**Namespace:** `UiPath.MicrosoftOffice365.Activities.Calendar.Models`

A composition argument object used by calendar activities to specify which calendar to operate on. The `InputMode` property determines which sub-properties are required.

## InputMode (ECalendarMode)

| Mode | Description | Required Properties | AI-XAML Suitable |
|------|-------------|--------------------|--------------------|
| `Browse` | Select a calendar via the Studio designer browser widget. | `BrowserCalendarId` (or `BrowserGroupId` for group calendars) | **Not suitable for AI-generated XAML** |
| `UseExisting` | Pass an existing [O365CalendarItem](../types/O365CalendarItem.md) reference, or leave null for the default calendar. | `Existing` (optional; null = default calendar) | Yes |

## Properties

### Core

| Property | Type | Description |
|----------|------|-------------|
| `InputMode` | `ECalendarMode` | The mode for specifying the calendar. |
| `ConnectionKey` | `String` | The connection ID from when the calendar was selected. |
| `ConnectionDescriptor` | `String` | A user-friendly label for the connection. |

### UseExisting Mode

| Property | Type | Description |
|----------|------|-------------|
| `Existing` | `InArgument<O365CalendarItem>` | An existing calendar item reference. Pass `null` or omit to use the default calendar. |

### Browse Mode (designer-only)

| Property | Type | Description |
|----------|------|-------------|
| `BrowserCalendarId` | `InArgument<String>` | Calendar ID from browser. `[AutopilotIgnored]` |
| `BrowserFriendlyName` | `InArgument<String>` | Display name from browser. |
| `BrowserCalendarGroupId` | `InArgument<String>` | Calendar group ID from browser. `[AutopilotIgnored]` |
| `BrowserGroupId` | `InArgument<String>` | Microsoft 365 group ID from browser. `[AutopilotIgnored]` |

### Other

| Property | Type | Description |
|----------|------|-------------|
| `Backup` | `BackupSlot<ECalendarMode>` | Stores the previous InputMode for undo. |

## XAML Examples

### UseExisting Mode (default calendar)
```xml
<calmodels:CalendarArgument InputMode="UseExisting" />
```

### UseExisting Mode (specific calendar)
```xml
<calmodels:CalendarArgument InputMode="UseExisting">
    <calmodels:CalendarArgument.Existing>
        <InArgument x:TypeArguments="calmodels2:O365CalendarItem">[myCalendar]</InArgument>
    </calmodels:CalendarArgument.Existing>
</calmodels:CalendarArgument>
```

## Cross-References

- Selects calendars of type [O365CalendarItem](../types/O365CalendarItem.md)
- Used by calendar activities (CreateEventConnections, GetEventListConnections, ForEachEventConnections, etc.)
- Events are of type [O365EventItem](../types/O365EventItem.md)
- Events can be filtered by [EventFilterCollection](../filtering/EventFilterCollection.md)
