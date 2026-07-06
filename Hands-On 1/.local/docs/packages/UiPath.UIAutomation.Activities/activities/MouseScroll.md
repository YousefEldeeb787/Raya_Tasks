# Mouse Scroll

`UiPath.UIAutomationNext.Activities.NMouseScroll`

Sends mouse scroll events to the specified UI element.

**Package:** `UiPath.UIAutomation.Activities`
**Category:** UI Automation.Application
**Required Scope:** `UiPath.UIAutomationNext.Activities.NApplicationCard`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Placeholder | Description |
|------|-------------|------|------|----------|---------|-------------|-------------|
| `Target` | Target | Property | [`TargetAnchorable`](common/Target.md#targetanchorable) |  |  |  | The UI element to perform the action on. |
| `CursorMotionType` | Cursor motion type | InArgument | [`CursorMotionType`](common/CursorMotionType.md) |  |  |  | Specifies the type of motion performed by the mouse cursor. There are two options: Instant - the cursor jumps to the destination, and Smooth - the cursor moves in increments. Setting has effect only if input method Hardware Events is used. The default option is Instant. |
| `InUiElement` | Input element | InArgument | [`UiElement`](common/UiElement.md) |  |  |  | The Input UI Element defines the screen element that the activity will be executed on. |
| `SearchInArgument` | Input searched element | InArgument | [`UiElement`](common/UiElement.md) |  |  |  | An existing UI element reference to use as the searched element. The mouse scroll continues until this element becomes visible. |

### Configuration

| Name | Display Name | Kind | Type | Default | Required | Description |
|------|-------------|------|------|---------|----------|-------------|
| `Direction` | Direction | InArgument | [`NScrollDirection`](common/NScrollDirection.md) |  |  | Specifies the type of scroll to be performed with the mouse wheel. |
| `MovementUnits` | # of scrolls | InArgument | `int` |  |  | The number of scrolls. If scrolling to a specific element, the element is searched after all rotations are executed. |
| `SearchedElement` | Searched element | Property | [`SearchedElement`](common/SearchedElement.md) |  |  | Define the element that must found and visible on screen while scrolling. |
| `KeyModifiers` | Key modifiers | InArgument | [`NKeyModifiers`](common/NKeyModifiers.md) |  |  | One or more key modifiers to use in combination with the mouse scroll action. |
| `InteractionMode` | Input mode | InArgument | [`NChildInteractionMode`](common/NChildInteractionMode.md) |  |  | The method used to execute the click. |
| `HealingAgentBehavior` | Healing Agent mode | InArgument | [`NChildHealingAgentBehavior`](common/NChildHealingAgentBehavior.md) |  |  | Configures the Healing Agent actions if they are allowed by Governance or Orchestrator process/job/trigger level settings |
| `ScrollType` | Scroll type | Property | [`NScrollType`](#nscrolltype) |  |  | Indicates the behavior of the mouse scroll activity. |

### Output

| Name | Display Name | Type | Description |
|------|-------------|------|-------------|
| `OutUiElement` | Output element | [`UiElement`](common/UiElement.md) | Output a UI Element to use in other activities as an Input UI Element. |
| `SearchOutArgument` | Output searched element | [`UiElement`](common/UiElement.md) | The searched UI element reference, to use in other activities as in Input UI Element. |

### Common

| Name | Display Name | Kind | Type | Default | Required | Description |
|------|-------------|------|------|---------|----------|-------------|
| `ContinueOnError` | Continue on error | InArgument | `bool` |  |  | Continue executing the activities in the automation if this activity fails. The default value is False. |
| `Timeout` | Timeout | InArgument | `double` |  |  | The amount of time (in seconds) to wait for the operation to be performed before generating an error. The default value is 30 seconds. |
| `DelayAfter` | Delay after | InArgument | `double` |  |  | Delay (in seconds) after this activity is completed, before next activity starts. The default amount of time is 0.3 seconds. |
| `DelayBefore` | Delay before | InArgument | `double` |  |  | Delay (in seconds) to wait before executing this activity. The default amount of time is 0.2 seconds. |

## Sub-Objects

See [`SearchedElement`](common/SearchedElement.md) for the full property table and XAML nested-element syntax.

## Enums

### NScrollType

`UiPath.UIAutomationNext.Enums.NScrollType`

How scrolling is performed by the **Mouse Scroll** activity.

| Value | Description |
|-------|-------------|
| `NScrollType.Distance` | Scroll by a configured distance. |
| `NScrollType.ToElement` | Scroll until a target element is reached. |

## How to create a new Mouse Scroll

To generate the default XAML for this activity, run the following command:

```bash
uip rpa get-default-activity-xaml --activity-class-name UiPath.UIAutomationNext.Activities.NMouseScroll
```
## Notes

- This activity must be placed inside a **Use Application/Browser** (`NApplicationCard`) scope.
- Supports scrolling in multiple directions (Up, Down, Left, Right).
- Use the `SearchedElement` property to scroll until a specific element becomes visible.
- The `KeyModifiers` property allows holding modifier keys (e.g., Ctrl, Shift) during scrolling, enabling horizontal scroll or zoom behavior.
