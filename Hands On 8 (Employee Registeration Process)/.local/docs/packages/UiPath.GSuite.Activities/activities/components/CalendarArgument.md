# CalendarArgument

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Activities.Calendar.Models.CalendarArgument`

A composition component used by Google Calendar activities to specify a target calendar. Supports browsing for a calendar or using an existing [GSuiteCalendarItem](../types/GSuiteCalendarItem.md) variable.

**Assembly:** `UiPath.GSuite.Activities`
**Inherits:** `BaseCalendarArgument`

## InputMode (`ECalendarMode`)

Determines which properties are active.

| Mode | Value | Description | AI-XAML Suitable |
|------|-------|-------------|------------------|
| `Browse` | `0` | Select a calendar from the remote browser in Studio. | **Not suitable for AI-generated XAML** -- requires interactive Studio UI. |
| `UseExisting` | `1` | Use an existing [`GSuiteCalendarItem`](../types/GSuiteCalendarItem.md) reference. | Yes |

## Properties

| Property | Type | Mode(s) | Description |
|----------|------|---------|-------------|
| `InputMode` | `ECalendarMode` | All | Determines how the calendar is specified. |
| `Existing` | `InArgument<`[`GSuiteCalendarItem`](../types/GSuiteCalendarItem.md)`>` | UseExisting | An existing calendar reference to use as the target. |
| `BrowserId` | `InArgument<string>` | Browse | The calendar identifier persisted after the user selects a calendar via the remote browser. |
| `FriendlyName` | `InArgument<string>` | Browse | The display name of the selected calendar (e.g., "My Calendar"). Used as a fallback to resolve by name when `BrowserId` is no longer valid after a connection change. |
| `ConnectionKey` | `string` | All | The identifier of the connection that was active when the calendar was selected. Used to detect connection changes for re-resolution. |
| `ConnectionDescriptor` | `string` | All | A human-readable label describing the connection (e.g., account email). |
| `Backup` | `BackupSlot<ECalendarMode>` | All | Stores the previous InputMode value so the designer can revert when switching modes. Studio **always** serializes this — every XAML example must include the `.Backup` child element. |

## XAML Examples

> **Supported `InputMode` values:** `UseExisting` *(only AI-suitable mode)*. `Browse` requires interactive Studio selection — not suitable for AI-generated XAML.

### UseExisting Mode (only AI-suitable mode)

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:models="clr-namespace:UiPath.GSuite.Activities.Calendar.Models;assembly=UiPath.GSuite.Activities"
    xmlns:calEnums="clr-namespace:UiPath.GSuite.Activities.Calendar.Enums;assembly=UiPath.GSuite.Activities"
    xmlns:calendar="clr-namespace:UiPath.GSuite.Calendar.Models;assembly=UiPath.GSuite"
    xmlns:usau="clr-namespace:UiPath.Shared.Activities.Utils;assembly=UiPath.GSuite.Activities"
    xmlns:scg="clr-namespace:System.Collections.Generic;assembly=mscorlib"
-->
<models:CalendarArgument InputMode="UseExisting">
  <models:CalendarArgument.Existing>
    <InArgument x:TypeArguments="calendar:GSuiteCalendarItem">[MyCalendarVariable]</InArgument>
  </models:CalendarArgument.Existing>
  <models:CalendarArgument.Backup>
    <usau:BackupSlot x:TypeArguments="calEnums:ECalendarMode" StoredValue="UseExisting">
      <usau:BackupSlot.BackupValues>
        <scg:Dictionary x:TypeArguments="calEnums:ECalendarMode, scg:List(x:Object)" />
      </usau:BackupSlot.BackupValues>
    </usau:BackupSlot>
  </models:CalendarArgument.Backup>
</models:CalendarArgument>
```

## Notes

- When no calendar is specified (empty `BrowserId` in Browse mode), the default calendar (primary) is used.
- When the connection changes, Browse mode attempts to re-resolve the calendar by verifying the `BrowserId`, then falls back to searching by `FriendlyName`.
- `BrowserId` has the `[AutopilotIgnored]` attribute -- it is a regular property but is typically populated by the Studio browser, not by users directly.
- Every XAML example that contains a `CalendarArgument` must include a `.Backup` child element with `StoredValue` matching the active `InputMode` and an empty `BackupValues` dictionary.

## Used By

Google Calendar activities that need a calendar reference -- see activity docs for details.
