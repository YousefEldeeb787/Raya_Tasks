# FileFilterArgument

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Activities.Drive.Filters.FileFilterArgument`

A filter collection for Google Drive file listing activities. Allows composing multiple filter conditions on file properties (name, type, dates, owner, text content, and Drive labels) with a logical operator to combine them.

**Assembly:** `UiPath.GSuite.Activities`
**Inherits:** `BaseFilterCollection<object, FileFilterLogicalOperator>`

## Structure

A `FileFilterArgument` contains:
- A `LogicalOperator` that determines how multiple filters combine (AND/OR).
- A `Filters` list containing filter elements (polymorphic: `SingleFileFilterArgument`, `LabelFilterArgument`, or `LabelFieldFilterArgument`).

## Properties

| Property | Type | Description |
|----------|------|-------------|
| `LogicalOperator` | `FileFilterLogicalOperator` | How to combine multiple filter conditions. |
| `Filters` | `List<object>` | The list of filter elements to apply. |

## FileFilterLogicalOperator Enum

| Value | Description |
|-------|-------------|
| `And` | All filters must match (intersection). |
| `Or` | Any filter may match (union). |

## Filter Element Types

### SingleFileFilterArgument

A filter on a standard file property.

| Property | Type | Description |
|----------|------|-------------|
| `Criteria` | `PropertyFilterField` | The field to filter on. |
| `StringOperator` | `StringFilterOperator` | Operator for short text fields (Name). |
| `TextOperator` | `TextFilterOperator` | Operator for long text fields (TextInFile). |
| `DateOperator` | `DateFilterOperator` | Operator for date fields (CreationDateTime, LastModifiedDateTime). |
| `OwnerOperator` | `CollectionFilterOperator` | Operator for owner fields. |
| `TypeOperator` | `FileTypeFilterOperator` | Operator for file type filtering. |
| `FileType` | `FileTypes` | The file type to match (used with Type criteria). |
| `Value` | `InArgument<string>` | String value for the comparison. Ignored for date criteria. |
| `Values` | `InArgument<string[]>` | Multiple string values for collection comparisons. |
| `DateValue` | `InArgument<DateTime>` | DateTime value for date comparisons. |

### LabelFilterArgument

A filter on the presence/absence of a Drive label.

| Property | Type | Description |
|----------|------|-------------|
| `Criteria` | `string` | The label criteria (Base64-encoded `GDriveLabelSlim`). |
| `LabelOperator` | `CollectionFilterOperator` | Operator for label membership checks. |

### LabelFieldFilterArgument

A filter on a specific field within a Drive label.

| Property | Type | Description |
|----------|------|-------------|
| `Criteria` | `string` | The label field criteria (Base64-encoded `GDriveLabelFieldSlim`). |
| `StringOperator` | `StringFilterOperator` | Operator for short text and single-selection fields. |
| `TextOperator` | `TextFilterOperator` | Operator for text-type label fields. |
| `DateOperator` | `DateFilterOperator` | Operator for date-type label fields. |
| `OwnerOperator` | `CollectionFilterOperator` | Operator for user-list and multi-selection fields. |
| `IntegerOperator` | `NumberFilterOperator` | Operator for number-type label fields. |
| `Value` | `InArgument<string>` | String value for comparisons. |
| `Values` | `InArgument<string[]>` | Multiple string values for collection comparisons. |
| `DateValue` | `InArgument<DateTime>` | DateTime value for date comparisons. |
| `IntegerValue` | `InArgument<int>` | Integer value for number comparisons. |

## PropertyFilterField Enum

| Value | Description |
|-------|-------------|
| `Name` | Filter by file name. |
| `TextInFile` | Filter by text content within the file. |
| `Type` | Filter by file type. |
| `CreationDateTime` | Filter by creation date and time. |
| `LastModifiedDateTime` | Filter by last modified date and time. |
| `Owner` | Filter by file owner. |

## Operator Enums

### StringFilterOperator

| Value | Description |
|-------|-------------|
| `Contains` | Value contains the specified text. |
| `NotContains` | Value does not contain the specified text. |
| `Equals` | Value equals the specified text. |
| `NotEquals` | Value does not equal the specified text. |
| `IsEmpty` | Value is empty (no operand needed). |
| `IsNotEmpty` | Value is not empty (no operand needed). |

### TextFilterOperator

| Value | Description |
|-------|-------------|
| `Contains` | Text contains the specified content. |
| `NotContains` | Text does not contain the specified content. |
| `IsEmpty` | Text is empty (no operand needed). |
| `IsNotEmpty` | Text is not empty (no operand needed). |

### DateFilterOperator

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

### FileTypeFilterOperator

| Value | Description |
|-------|-------------|
| `Equals` | File type is the specified type. |
| `NotEquals` | File type is not the specified type. |

### NumberFilterOperator

| Value | Description |
|-------|-------------|
| `Equals` | Number equals the specified value. |
| `NotEquals` | Number does not equal the specified value. |
| `GreaterThan` | Number is greater than the specified value. |
| `GreaterOrEqualThan` | Number is greater than or equal to the specified value. |
| `LessThan` | Number is less than the specified value. |
| `LessOrEqualThan` | Number is less than or equal to the specified value. |
| `IsEmpty` | Number is not set (no operand needed). |
| `IsNotEmpty` | Number is set (no operand needed). |

### CollectionFilterOperator

| Value | Description |
|-------|-------------|
| `In` | Any of the specified values are in the collection. |
| `NotIn` | None of the specified values are in the collection. |
| `AllIn` | All of the specified values are in the collection. |
| `NotAllIn` | At least some of the specified values are NOT in the collection. |
| `IsEmpty` | Collection is empty (no operand needed). |
| `IsNotEmpty` | Collection is not empty (no operand needed). |

### FileTypes Enum

| Value | Description |
|-------|-------------|
| `GoogleDocs` | Google Docs |
| `PDF` | PDF files |
| `ZIP` | ZIP archives |
| `PlainText` | Plain text files |
| `RichText` | Rich text files |
| `MSWord` | Microsoft Word documents |
| `OpenOfficeDoc` | OpenOffice documents |
| `GoogleSlides` | Google Slides |
| `MSPowerPoint` | Microsoft PowerPoint presentations |
| `OpenOfficePresentation` | OpenOffice presentations |
| `GoogleSpreadsheet` | Google Sheets |
| `MSExcel` | Microsoft Excel spreadsheets |
| `OpenOfficeSheet` | OpenOffice spreadsheets |
| `CSV` | CSV files |
| `Images` | Image files |
| `GoogleDrawing` | Google Drawings |
| `Videos` | Video files |
| `Audio` | Audio files |

## Used By

Google Drive file listing/iteration activities -- see activity docs for details.
