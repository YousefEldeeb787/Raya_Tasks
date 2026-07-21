# GFormInfo

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Forms.Models.GFormInfo`

Represents a Google Form with its metadata, including linked sheet information and a responder URI. Inherits all Google Drive file properties from [GDriveRemoteItem](GDriveRemoteItem.md).

**Assembly:** `UiPath.GSuite`
**Inherits:** [`GDriveRemoteItem`](GDriveRemoteItem.md)

## Properties

### GFormInfo-Specific Properties

| Property | Type | Description |
|----------|------|-------------|
| `FormId` | `string` | The unique identifier of the Google Form. |
| `Title` | `string` | The title of the form. |
| `Description` | `string` | The description of the form. |
| `LinkedSheetId` | `string` | The ID of the linked Google Sheet which is accumulating responses from this Form (if such a Sheet exists). |
| `ResponderUri` | `string` | The form URI to share with responders. Opens a page that allows users to submit responses but not edit questions. |
| `Questions` | `GFormQuestionCollection` | The collection of questions in the form. |

### Inherited from [GDriveRemoteItem](GDriveRemoteItem.md)

All properties from [GDriveRemoteItem](GDriveRemoteItem.md) are available, including `Name`, `ID`, `Url`, `CreationDate`, `LastModifiedDate`, `MimeType`, `IsFolder`, `ParentId`, `CreatedBy`, `LastModifiedBy`, and others.

## Notes

- Since this type extends [GDriveRemoteItem](GDriveRemoteItem.md), it can be used anywhere a [GDriveRemoteItem](GDriveRemoteItem.md) is accepted.
- The `LinkedSheetId` is only populated if the form has a linked Google Sheet for response collection.
- The `ResponderUri` is the URL you would share with people who should fill out the form.

## Used By

Activities that return or accept this type -- see activity docs for details.
