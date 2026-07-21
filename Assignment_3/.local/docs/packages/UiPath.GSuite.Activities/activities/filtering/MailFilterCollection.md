# MailFilterCollection

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Activities.Gmail.Filters.MailFilterCollection`

A filter collection for Gmail email listing activities. Allows composing multiple filter conditions on email properties (sender, recipients, subject, body, dates, labels, categories, filename) with a logical operator to combine them.

**Assembly:** `UiPath.GSuite.Activities`
**Inherits:** `BaseFilterCollection<MailFilterElement, MailFilterLogicalOperator>`

## Structure

A `MailFilterCollection` contains:
- A `LogicalOperator` that determines how multiple filters combine (AND/OR).
- A `Filters` list of `MailFilterElement` instances.

## Properties

| Property | Type | Description |
|----------|------|-------------|
| `LogicalOperator` | `MailFilterLogicalOperator` | How to combine multiple filter conditions. |
| `Filters` | `List<MailFilterElement>` | The list of filter elements to apply. |

## MailFilterLogicalOperator Enum

| Value | Description |
|-------|-------------|
| `And` | All filters must match (intersection). |
| `Or` | Any filter may match (union). |

## MailFilterElement

Each element represents a single filter condition.

| Property | Type | Description |
|----------|------|-------------|
| `Criteria` | `MailFilterField` | The email field to filter on. |
| `StringOperator` | `MailStringFilterOperator` | Operator for string fields (From, To, CC, BCC, Subject, Body, Filename). |
| `DateOperator` | `MailDateFilterOperator` | Operator for the DateAndTime field. |
| `CollectionFilterOperator` | `CollectionFilterOperator` | Operator for collection fields (Labels). |
| `Value` | `InArgument<string>` | String value for comparison. Ignored for date criteria. |
| `DateEqualsFilter` | `InArgument<DateTime>` | DateTime value for date comparisons. |
| `Values` | `InArgument<string[]>` | Collection of values for label filtering. |

## MailFilterField Enum

| Value | Description |
|-------|-------------|
| `From` | Filter by sender. |
| `To` | Filter by recipient. |
| `DateAndTime` | Filter by date and time. |
| `CC` | Filter by CC recipient. |
| `BCC` | Filter by BCC recipient. |
| `Subject` | Filter by subject line. |
| `Body` | Filter by message body. |
| `Categories` | Filter by Gmail categories. |
| `Filename` | Filter by attachment filename. |
| `Labels` | Filter by Gmail labels. |

## Operator Enums

### MailStringFilterOperator

| Value | Description |
|-------|-------------|
| `Contains` | Value contains the specified text. |
| `NotContains` | Value does not contain the specified text. |
| `Equals` | Value equals the specified text. |
| `StartsWith` | Value starts with the specified text. |
| `EndsWith` | Value ends with the specified text. |
| `IsEmpty` | Value is empty (no operand needed). |
| `IsNotEmpty` | Value is not empty (no operand needed). |

### MailDateFilterOperator

| Value | Description |
|-------|-------------|
| `NewerThan` | Date is newer than the specified date. |
| `OlderThan` | Date is older than the specified date. |

### CollectionFilterOperator

| Value | Description |
|-------|-------------|
| `In` | Any of the specified values are in the collection. |
| `NotIn` | None of the specified values are in the collection. |
| `AllIn` | All of the specified values are in the collection. |
| `NotAllIn` | At least some of the specified values are NOT in the collection. |
| `IsEmpty` | Collection is empty (no operand needed). |
| `IsNotEmpty` | Collection is not empty (no operand needed). |

## Notes

- Date filtering for Gmail only supports `NewerThan` and `OlderThan` (no exact equals).
- The `Labels` criteria uses collection operators to filter by Gmail label membership.
- The `Categories` criteria filters by Gmail category tabs (Primary, Social, Promotions, Updates, Forums).

## Used By

Gmail email listing/iteration activities -- see activity docs for details.
