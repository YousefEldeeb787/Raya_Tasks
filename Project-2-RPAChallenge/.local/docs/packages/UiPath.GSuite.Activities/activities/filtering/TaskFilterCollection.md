# TaskFilterCollection

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Activities.Tasks.Filters.TaskFilterCollection`

A filter collection for Google Tasks listing activities. Allows composing multiple filter conditions on task properties (title, details, due date, completed date) with a logical operator to combine them.

**Assembly:** `UiPath.GSuite.Activities`
**Inherits:** `BaseFilterCollection<TaskFilterArgument, TaskFilterLogicalOperator>`

## Structure

A `TaskFilterCollection` contains:
- A `LogicalOperator` that determines how multiple filters combine (AND/OR).
- A `Filters` list of `TaskFilterArgument` instances.

## Properties

| Property | Type | Description |
|----------|------|-------------|
| `LogicalOperator` | `TaskFilterLogicalOperator` | How to combine multiple filter conditions. |
| `Filters` | `List<TaskFilterArgument>` | The list of filter elements to apply. |

## TaskFilterLogicalOperator Enum

| Value | Description |
|-------|-------------|
| `And` | All filters must match (intersection). |
| `Or` | Any filter may match (union). |

## TaskFilterArgument

Each element represents a single filter condition on a task property.

| Property | Type | Description |
|----------|------|-------------|
| `Criteria` | `TaskFilterField` | The task field to filter on. |
| `StringOperator` | `TaskStringFilterOperator` | Operator for string fields (Title, Details). |
| `DateOperator` | `TaskDateFilterOperator` | Operator for date fields (DueDate, CompletedDate). |
| `Value` | `InArgument<string>` | String value for comparison. Ignored for date criteria. |
| `DateValue` | `InArgument<DateTime>` | DateTime value for date comparisons. |

## TaskFilterField Enum

| Value | Description |
|-------|-------------|
| `Title` | Filter by task title. |
| `Details` | Filter by task details/notes. |
| `DueDate` | Filter by due date. |
| `CompletedDate` | Filter by completion date. |

## Operator Enums

### TaskStringFilterOperator

| Value | Description |
|-------|-------------|
| `Contains` | Value contains the specified text. |
| `NotContains` | Value does not contain the specified text. |
| `Equals` | Value equals the specified text. |
| `NotEquals` | Value does not equal the specified text. |
| `StartsWith` | Value starts with the specified text. |
| `NotStartsWith` | Value does not start with the specified text. |
| `EndsWith` | Value ends with the specified text. |
| `NotEndsWith` | Value does not end with the specified text. |
| `IsEmpty` | Value is empty (no operand needed). |
| `IsNotEmpty` | Value is not empty (no operand needed). |

### TaskDateFilterOperator

| Value | Description |
|-------|-------------|
| `NewerThan` | Date is newer than the specified date. |
| `OlderThan` | Date is older than the specified date. |
| `Equals` | Date equals the specified date. |
| `NotEquals` | Date does not equal the specified date. |
| `NewerOrEqualThan` | Date is newer than or equal to the specified date. |
| `OlderOrEqualThan` | Date is older than or equal to the specified date. |
| `IsEmpty` | Date is not set (no operand needed). |
| `IsNotEmpty` | Date is set (no operand needed). |

## Used By

Google Tasks listing/iteration activities -- see activity docs for details.
