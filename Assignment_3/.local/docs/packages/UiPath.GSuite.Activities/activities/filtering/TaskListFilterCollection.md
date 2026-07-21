# TaskListFilterCollection

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Activities.Tasks.Filters.TaskListFilterCollection`

A filter collection for Google Task Lists listing activities. Allows composing filter conditions on task list properties (currently only title) with a logical operator to combine them.

**Assembly:** `UiPath.GSuite.Activities`
**Inherits:** `BaseFilterCollection<TaskListFilterArgument, TaskFilterLogicalOperator>`

## Structure

A `TaskListFilterCollection` contains:
- A `LogicalOperator` that determines how multiple filters combine (AND/OR).
- A `Filters` list of `TaskListFilterArgument` instances.

## Properties

| Property | Type | Description |
|----------|------|-------------|
| `LogicalOperator` | `TaskFilterLogicalOperator` | How to combine multiple filter conditions. |
| `Filters` | `List<TaskListFilterArgument>` | The list of filter elements to apply. |

## TaskFilterLogicalOperator Enum

| Value | Description |
|-------|-------------|
| `And` | All filters must match (intersection). |
| `Or` | Any filter may match (union). |

## TaskListFilterArgument

Each element represents a single filter condition on a task list property.

| Property | Type | Description |
|----------|------|-------------|
| `Criteria` | `TaskListFilterField` | The task list field to filter on. |
| `StringOperator` | `TaskStringFilterOperator` | The string comparison operator. |
| `Value` | `InArgument<string>` | String value for comparison. |

## TaskListFilterField Enum

| Value | Description |
|-------|-------------|
| `Title` | Filter by task list title. |

## TaskStringFilterOperator Enum

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

## Used By

Google Tasks list listing/iteration activities -- see activity docs for details.
