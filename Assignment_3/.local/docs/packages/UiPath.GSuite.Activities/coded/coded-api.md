# UiPath.GSuite.Activities — Coded Workflow API

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating C# code. API signatures, return types, and service method patterns are defined in the linked type and service docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Activities`

**Service accessor:** `google`
**Service type:** `IGoogleConnectionsService`

## Service Overview

Coded workflow API for automating Google Workspace services. The `google` service accessor uses a **connection-based pattern** — each Google Workspace service requires its own typed connection object.

| Sub-service | Method | Connection type | Description |
|-------------|--------|-----------------|-------------|
| `IGmailService` | `google.Gmail(conn)` | `GmailConnection` | Send, receive, and manage Gmail messages. |
| `IGoogleCalendarService` | `google.Calendar(conn)` | `GmailConnection` | Create and manage calendar events. |
| `IGoogleDriveService` | `google.Drive(conn)` | `DriveConnection` | Manage files and folders in Google Drive. |
| `IGoogleSheetsService` | `google.Sheets(conn)` | `SheetsConnection` | Read and write Google Sheets spreadsheets. |
| `IGoogleDocsService` | `google.Docs(conn)` | `DocsConnection` | Read and write Google Docs documents. |
| `IGoogleTasksService` | `google.Tasks(conn)` | `TasksConnection` | Manage Google Tasks and task lists. |
| `IFormsService` | `google.Forms(conn)` | `FormsConnection` | Retrieve Google Forms info and responses. |
| `IGoogleWorkspaceService` | `google.Workspace(conn)` | `WorkspaceConnection` | Make authenticated HTTP requests and run Apps Script. |

## Auto-Imported Namespaces

The following namespace is automatically imported in coded workflows using this package:

- `UiPath.GSuite.Activities.Api`

---

## API Pattern: Connection-Based

This API uses a **connection-based pattern**. Each Google Workspace service requires its own typed connection object. You obtain a sub-service by passing the corresponding connection to the top-level `google` accessor:

```csharp
var gmail = google.Gmail(new GmailConnection("myGmailConnectionId"));
var sheets = google.Sheets(new SheetsConnection("mySheetsConnectionId"));
var drive = google.Drive(new DriveConnection("myDriveConnectionId"));
```

The connection ID corresponds to a connection configured in UiPath Automation Cloud or Orchestrator.

---

## Required Package Dependency

Add to `project.json`:
```json
{
  "dependencies": {
    "UiPath.GSuite.Activities": "[version]"
  }
}
```

---

## Connection Types

Each connection wraps a Google OAuth credential stored in UiPath Connections.

| Type | Description |
|------|-------------|
| `GmailConnection(string connectionId)` | Connection to Gmail. |
| `SheetsConnection(string connectionId)` | Connection to Google Sheets. |
| `DriveConnection(string connectionId)` | Connection to Google Drive. |
| `DocsConnection(string connectionId)` | Connection to Google Docs. |
| `TasksConnection(string connectionId)` | Connection to Google Tasks. |
| `FormsConnection(string connectionId)` | Connection to Google Forms. |
| `WorkspaceConnection(string connectionId)` | Connection to Google Workspace (HTTP requests, Apps Script). |

---

## `IGoogleConnectionsService` — Top-Level Service

Accessed via the `google` variable in coded workflows.

### `IGmailService Gmail(GmailConnection connection)`

Returns a Gmail service for the given connection.

### `IGoogleCalendarService Calendar(GmailConnection connection)`

Returns a Google Calendar service. Uses the Gmail connection (Calendar shares Google account auth with Gmail).

### `IGoogleSheetsService Sheets(SheetsConnection connection)`

Returns a Google Sheets service for the given connection.

### `IGoogleDriveService Drive(DriveConnection connection)`

Returns a Google Drive service for the given connection.

### `IGoogleDocsService Docs(DocsConnection connection)`

Returns a Google Docs service for the given connection.

### `IGoogleTasksService Tasks(TasksConnection connection)`

Returns a Google Tasks service for the given connection.

### `IFormsService Forms(FormsConnection connection)`

Returns a Google Forms service for the given connection.

### `IGoogleWorkspaceService Workspace(WorkspaceConnection connection)`

Returns a Google Workspace service (HTTP requests and Apps Script) for the given connection.

---

## `IGmailService` — Gmail

Obtained via `google.Gmail(new GmailConnection("connId"))`.

### Properties

#### `MailSystemFolders SystemFolders`

Provides access to well-known Gmail system label folders (Inbox, Sent, Trash, etc.). See [`MailSystemFolders`](#mailsystemfolders).

### Methods

#### `IReadOnlyCollection<IMailLabel> GetMailLabels()`

Retrieves all Gmail labels (system and user-defined) for the authenticated account.

**Returns:** Collection of `IMailLabel`.

---

#### `IMail GetNewestEmail(IMailLabel folder = null, MailFilter filter = null, bool markAsRead = false)`

Retrieves the most recent email matching the filter from the specified folder.

**Parameters:**
- `folder` — Label folder to search in. Defaults to All Mail (`SystemFolders.AllMail`).
- `filter` — Filter criteria. See [`MailFilter`](#mailfilter).
- `markAsRead` — Whether to mark the retrieved email as read. Default: `false`.

**Returns:** `IMail` — the matching email, or `null` if none found.

---

#### `IMail GetEmail(string emailId)`

Retrieves a specific email by its ID.

**Returns:** `IMail`.

---

#### `IReadOnlyCollection<IMail> GetEmails(IMailLabel folder = null, MailFilter filter = null, bool markAsRead = false, int? maxResults = null)`

Retrieves emails matching the filter from the specified folder.

**Parameters:**
- `folder` — Label folder to search in. Defaults to All Mail.
- `filter` — Filter criteria.
- `markAsRead` — Whether to mark retrieved emails as read.
- `maxResults` — Maximum number of results. `null` means unlimited.

**Returns:** Collection of `IMail`.

---

#### `void SendEmail(string to, string subject, string body, string cc = null, string bcc = null, bool asDraft = false)`

Sends an email with the specified recipients and content.

**Parameters:**
- `to` — Primary recipient email address.
- `subject` — Email subject.
- `body` — Email body content (plain text).
- `cc` — CC recipient. Optional.
- `bcc` — BCC recipient. Optional.
- `asDraft` — Save as draft instead of sending. Default: `false`.

---

#### `void SendEmail(SendEmailRequest sendEmailOptions)`

Sends an email using a fluent request builder. Supports HTML bodies, multiple recipients, attachments, importance, and reply-to address. See [`SendEmailRequest`](#sendemailrequest).

---

#### `void ReplyToEmail(IMail mail, string body, string newSubject = null, string to = null, string cc = null, string bcc = null, bool asDraft = false)`

Replies to an email.

---

#### `void ReplyToEmail(IMail mail, ReplyToEmailRequest replyOptions)`

Replies to an email using a fluent request builder. See [`ReplyToEmailRequest`](#replytoemailrequest).

---

#### `void ForwardEmail(IMail mail, string body, string newSubject = null, string to = null, string cc = null, string bcc = null, bool asDraft = false)`

Forwards an email.

---

#### `void ForwardEmail(IMail mail, ForwardEmailRequest forwardOptions)`

Forwards an email using a fluent request builder. See [`ForwardEmailRequest`](#forwardemailrequest).

---

#### `void MoveEmail(IMail mail, IMailLabel source, IMailLabel destination)`

Moves an email from the source label to the destination label.

---

#### `void ArchiveEmail(IMail mail)` / `void ArchiveEmail(string messageId)`

Archives an email by removing it from the Inbox label.

---

#### `void DeleteEmail(IMail mail, bool deletePermanently = false)` / `void DeleteEmail(string messageId, bool deletePermanently = false)`

Deletes an email. Moves to Trash unless `deletePermanently` is `true`.

---

#### `Stream DownloadEmail(IMail mail)` / `Stream DownloadEmail(string messageId)`

Downloads an email as an EML stream.

**Returns:** `Stream` containing the email data.

---

#### `IReadOnlyCollection<IMailAttachmentInfo> GetEmailAttachmentsInfo(IMail mail)` / `GetEmailAttachmentsInfo(string messageId)`

Retrieves attachment metadata for an email.

**Returns:** Collection of `IMailAttachmentInfo`.

---

#### `Stream DownloadEmailAttachment(IMailAttachmentInfo info)`

Downloads a specific attachment.

**Returns:** `Stream` containing the attachment data.

---

#### `IReadOnlyDictionary<IMailAttachmentInfo, Stream> DownloadEmailAttachments(IMail mail)` / `DownloadEmailAttachments(string messageId)`

Downloads all attachments from an email.

**Returns:** Dictionary mapping attachment info to stream data.

---

#### `void MarkEmailAsRead(IMail mail)` / `void MarkEmailAsRead(string messageId)`

Marks an email as read.

---

#### `void MarkEmailAsUnread(IMail mail)` / `void MarkEmailAsUnread(string messageId)`

Marks an email as unread.

---

#### `void ApplyLabels(IMail mail, IReadOnlyList<IMailLabel> labels)`

Applies one or more labels to an email.

---

#### `void RemoveLabels(IMail mail, IReadOnlyList<IMailLabel> labels)`

Removes one or more labels from an email.

---

#### `void TurnOnAutomaticReplies(string subject, string body, DateTimeOffset startTime, DateTimeOffset endTime, bool sendRepliesOutsideOrganization = false, bool sendRepliesToContactsOnly = false)`

Enables the Gmail vacation responder (automatic replies).

---

#### `void TurnOffAutomaticReplies()`

Disables the Gmail vacation responder.

---

#### `IReadOnlyCollection<IMail> GetEmailThread(IMail mail)` / `GetEmailThread(string threadId)`

Retrieves all messages in a conversation thread.

**Returns:** Collection of `IMail`.

---

#### `IMailLabel GetLabel(string labelId)`

Retrieves a Gmail label by its ID.

**Returns:** `IMailLabel`.

---

## `IGoogleCalendarService` — Google Calendar

Obtained via `google.Calendar(new GmailConnection("connId"))`.

### Methods

#### `IReadOnlyCollection<ICalendar> GetCalendars()`

Retrieves all calendars for the authenticated account.

**Returns:** Collection of `ICalendar`.

---

#### `ICalendar GetDefaultCalendar()`

Retrieves the user's primary calendar.

**Returns:** `ICalendar`.

---

#### `IReadOnlyCollection<ICalendarEvent> GetEvents(DateTimeOffset startDate, DateTimeOffset endDate, ICalendar calendar = null, int top = 0, string timezone = "", string search = "")`

Retrieves events within a date range.

**Parameters:**
- `startDate` — Start of the search window.
- `endDate` — End of the search window.
- `calendar` — Calendar to search. Defaults to the primary calendar.
- `top` — Maximum events to return. `0` means unlimited.
- `timezone` — Timezone for event times. Defaults to UTC.
- `search` — Text filter applied to summary, description, attendees, and location.

**Returns:** Collection of `ICalendarEvent`.

---

#### `ICalendarEvent GetEventById(string eventId, ICalendar calendar = null)`

Retrieves a calendar event by its ID.

**Returns:** `ICalendarEvent`.

---

#### `ICalendarEvent CreateEvent(ICalendar calendar, CreateCalendarItem eventInformation)`

Creates a new event on the specified calendar. Use [`CreateCalendarItem`](#createcalendaritem) to build the event details.

**Returns:** The newly created `ICalendarEvent`.

---

#### `ICalendarEvent UpdateEvent(ICalendarEvent calendarEvent, UpdateCalendarItem changes)`

Updates an existing event. Use [`UpdateCalendarItem`](#updatecalendaritem) to specify changes.

**Returns:** The updated `ICalendarEvent`.

---

#### `void DeleteEvent(ICalendarEvent calendarEvent, DeleteEventMode deleteMode = DeleteEventMode.SingleEvent)`

Cancels an event and notifies all attendees. See [`DeleteEventMode`](#deleteeventmode).

---

#### `void ForwardEvent(ICalendarEvent calendarEvent, IEnumerable<string> attendees, bool forwardSeries = false)`

Forwards an event invitation to additional recipients.

**Parameters:**
- `forwardSeries` — Whether to forward all occurrences of a recurring event.

---

#### `void RespondToEvent(ICalendarEvent calendarEvent, EventResponseType response, string comment = "", int? additionalGuests = null, bool allOccurrences = false, bool sendResponseNotification = false)`

Accepts, tentatively accepts, or declines a calendar event invitation. See [`EventResponseType`](#eventresponsetype).

---

## `IGoogleDriveService` — Google Drive

Obtained via `google.Drive(new DriveConnection("connId"))`.

### File and Folder Retrieval

#### `IReadOnlyCollection<IDriveItem> GetFilesAndFolders(IFolder parent = null, DriveItemFilter filter = null, int maxResults = 50)`

Lists files and folders in a Drive folder.

**Returns:** Collection of `IDriveItem`.

---

#### `IReadOnlyCollection<IFolder> GetFolders(IFolder parent = null, DriveItemFilter filter = null, int maxResults = 50)`

Lists folders in a Drive folder.

**Returns:** Collection of `IFolder`.

---

#### `IReadOnlyCollection<IFile> GetFiles(IFolder parent = null, DriveItemFilter filter = null, int maxResults = 50)`

Lists files in a Drive folder.

**Returns:** Collection of `IFile`.

---

#### `IDriveItem GetItem(string identifier, IdentificationType type = IdentificationType.UrlOrId)` / `GetItem(IFolder parent, string relativePath)`

Retrieves a file or folder by URL/ID or relative path.

**Returns:** `IDriveItem`.

---

#### `IFile GetFile(string identifier, IdentificationType type = IdentificationType.UrlOrId)` / `GetFile(IFolder parent, string relativePath)`

Retrieves a file by URL/ID or relative path.

**Returns:** `IFile`.

---

#### `IFolder GetFolder(string identifier = null, IdentificationType type = IdentificationType.UrlOrId)` / `GetFolder(IFolder parent, string relativePath)`

Retrieves a folder by URL/ID, relative path, or root (pass `null` for root).

**Returns:** `IFolder`.

---

#### `IDriveItem RefreshItem(IDriveItem item)` / `IFile RefreshFile(IFile file)` / `IFolder RefreshFolder(IFolder folder)`

Fetches the latest version of a Drive item.

---

#### `IDriveItemInfo GetFileFolderInfo(string identifier, IdentificationType type = IdentificationType.UrlOrId)`

Retrieves detailed metadata for a file or folder by URL/ID or full path.

**Returns:** `IDriveItemInfo`.

---

### File Operations

#### `IFolder CreateFolder(string name, string description = null, IFolder parent = null, ConflictBehavior conflictBehavior = ConflictBehavior.AddSeparate)`

Creates a new folder in Drive.

**Returns:** The newly created `IFolder`.

---

#### `IFile CopyFile(IFile file, IFolder destination, string newName = null, ConflictBehavior conflictBehavior = ConflictBehavior.AddSeparate)`

Copies a file to a destination folder.

**Returns:** The copied `IFile`.

---

#### `IFile MoveFile(IFile file, IFolder destination, string newName = null, ConflictBehavior conflictBehavior = ConflictBehavior.AddSeparate)`

Moves a file to a destination folder.

**Returns:** The moved `IFile`.

---

#### `IFolder MoveFolder(IFolder folder, IFolder destination, string newName = null, ConflictBehavior conflictBehavior = ConflictBehavior.AddSeparate)`

Moves a folder to a destination folder.

**Returns:** The moved `IFolder`.

---

#### `IDriveItem RenameItem(IDriveItem item, string newName, RenameConflictBehavior conflictBehavior = RenameConflictBehavior.AddSeparate)`

Renames a file or folder.

**Returns:** The renamed `IDriveItem`.

---

#### `void DeleteItem(IDriveItem item)` / `void DeleteItem(string urlOrId)`

Permanently deletes a file or folder.

---

#### `Stream DownloadFile(IFile file, DownloadOptions options = null)`

Downloads a file as a stream. For Google Workspace file types (Docs, Sheets, Slides), specify export formats in `DownloadOptions`. See [`DownloadOptions`](#downloadoptions).

**Returns:** `Stream`.

---

#### `IFile UploadFile(IResource file, IFolder destination, bool convertToGoogleTypes = false, ConflictBehavior conflictBehavior = ConflictBehavior.AddSeparate, bool uploadAsIsIfConvertFails = false)`

Uploads a file resource to Drive.

---

#### `IFile UploadFile(string path, IFolder destination, bool convertToGoogleTypes = false, ConflictBehavior conflictBehavior = ConflictBehavior.AddSeparate, bool uploadAsIsIfConvertFails = false)`

Uploads a local file by path to Drive.

---

#### `IFile UploadFile(Stream stream, string fileName, IFolder destination, bool convertToGoogleTypes = false, ConflictBehavior conflictBehavior = ConflictBehavior.AddSeparate, bool uploadAsIsIfConvertFails = false)`

Uploads a stream as a file to Drive.

---

#### `IReadOnlyCollection<IFile> UploadFiles(IEnumerable<IResource> files, IFolder destination, ...)` / `UploadFiles(IEnumerable<string> paths, IFolder destination, ...)`

Uploads multiple files to Drive.

**Returns:** Collection of uploaded `IFile`.

---

### Sharing and Permissions

#### `string ShareFile(IFile file, Role role = Role.Reader, bool useDomainAdminAccess = false)`

Shares a file with anyone (public link).

**Returns:** The web URL of the shared file.

---

#### `string ShareFile(IFile file, string domain, Role role = Role.Reader, bool useDomainAdminAccess = false)`

Shares a file with a specific domain.

**Returns:** The web URL of the shared file.

---

#### `string ShareFile(IFile file, GranteeType shareWith, string recipients, bool sendNotificationEmail = true, Role role = Role.Reader, bool useDomainAdminAccess = false)`

Shares a file with specific users or groups.

**Returns:** The web URL of the shared file.

---

#### `string ShareFolder(IFolder folder, Role role = Role.Reader, bool useDomainAdminAccess = false)` / `ShareFolder(IFolder folder, string domain, ...)` / `ShareFolder(IFolder folder, GranteeType shareWith, string recipients, ...)`

Shares a folder. Same overloads as `ShareFile`.

---

#### `IReadOnlyCollection<GDriveItemPermission> GetItemPermissions(IDriveItem item, bool useDomainAdminAccess = false)`

Retrieves all sharing permissions for a file or folder.

---

#### `GDriveItemPermission UpdateItemPermission(IDriveItem item, string permissionId, Role role, DateTime? expirationTime = null, bool removeExpiration = false, bool useDomainAdminAccess = false)`

Updates a permission on a file or folder.

---

#### `void DeleteItemPermission(IDriveItem item, string permissionId, bool useDomainAdminAccess = false)`

Removes a sharing permission from a file or folder.

---

### Drive Labels

#### `IReadOnlyCollection<ILabel> GetDriveLabels(DriveLabelType driveLabelType = DriveLabelType.All)`

Retrieves available Drive labels in the organization.

**Returns:** Collection of `ILabel`.

---

#### `IReadOnlyCollection<ILabel> GetFileLabels(IFile file)`

Retrieves Drive labels applied to a file (including field values).

**Returns:** Collection of `ILabel`.

---

#### `void ApplyFileLabels(IFile file, IEnumerable<ILabel> labels)`

Applies Drive labels to a file.

---

#### `void RemoveFileLabels(IFile file, IEnumerable<ILabel> labels)`

Removes labels from a file.

---

#### `void ClearFileLabelFields(IFile file, string labelId, IEnumerable<GDriveLabelField> fields)`

Clears field values from a label applied to a file.

---

## `IGoogleSheetsService` — Google Sheets

Obtained via `google.Sheets(new SheetsConnection("connId"))`.

### Spreadsheet Operations

#### `IReadOnlyCollection<ISpreadsheet> GetSpreadsheets(GDriveRemoteItem parentFolder = null)`

Lists spreadsheets in a Drive folder.

**Returns:** Collection of `ISpreadsheet`.

---

#### `ISpreadsheet AddSpreadsheet(GDriveRemoteItem parentFolder, string spreadsheetName, string firstSheetName = "Sheet1", ConflictBehavior conflictResolution = ConflictBehavior.Fail)`

Creates a new spreadsheet in a Drive folder.

**Returns:** The new `ISpreadsheet`.

---

#### `IReadOnlyCollection<IRange> GetRanges(ISpreadsheet spreadsheet)`

Gets all sheets and named ranges in a spreadsheet.

**Returns:** Collection of `IRange`.

---

#### `IReadOnlyCollection<ISheet> GetSheets(ISpreadsheet spreadsheet)`

Gets all sheets (tabs) in a spreadsheet.

**Returns:** Collection of `ISheet`.

---

#### `IReadOnlyCollection<INamedRange> GetNamedRanges(ISpreadsheet spreadsheet)`

Gets all named ranges in a spreadsheet.

**Returns:** Collection of `INamedRange`.

---

#### `ISheet AddSheet(ISpreadsheet spreadsheet, string sheetName = null, int? positionIndex = null)`

Adds a new sheet to a spreadsheet.

**Returns:** The new `ISheet`.

---

#### `void RenameSheet(ISpreadsheet spreadsheet, string replacedSheetName, string newSheetName)`

Renames a sheet.

---

#### `void DeleteSheet(ISpreadsheet spreadsheet, string sheetName)`

Deletes a sheet from a spreadsheet.

---

### Reading and Writing Data

#### `DataTable ReadRange(ISpreadsheet spreadsheet, IRange range, bool hasHeaders = true, CellReadMode cellReadMode = CellReadMode.Values)`

Reads data from a range into a `DataTable`.

**Parameters:**
- `hasHeaders` — Whether the first row is a header row.
- `cellReadMode` — Whether to read values, formulas, or formatted text. See [`CellReadMode`](#cellreadmode).

**Returns:** `DataTable`.

---

#### `DataTable ReadRange(ISpreadsheet spreadsheet, IRange range, out IRangeInformation rangeInformation, bool hasHeaders = true, CellReadMode cellReadMode = CellReadMode.Values)`

Reads data from a range, also returning range metadata.

---

#### `void WriteRange(ISpreadsheet spreadsheet, IRange range, DataTable data, bool hasHeaders = true, RangeWriteMode writeMode = RangeWriteMode.Overwrite, int insertRowPosition = 0)`

Writes a `DataTable` to a range. See [`RangeWriteMode`](#rangewritemode).

---

#### `object ReadCell(ISpreadsheet spreadsheet, IRange range, string cell, CellReadMode cellReadMode = CellReadMode.Values)`

Reads a single cell value (e.g., `"A1"`).

**Returns:** The cell value as `object`.

---

#### `void WriteCell(ISpreadsheet spreadsheet, IRange range, string cell, object value)`

Writes a value to a single cell.

---

#### `DataRow ReadRow(ISpreadsheet spreadsheet, IRange range)`

Reads the first row from a range.

**Returns:** `DataRow`.

---

#### `object[] ReadColumn(ISpreadsheet spreadsheet, IRange range)`

Reads the first column from a range.

**Returns:** `object[]`.

---

#### `void WriteRow(ISpreadsheet spreadsheet, IRange range, DataRow data, bool hasHeaders = true, RangeWriteMode writeMode = RangeWriteMode.Append, int insertRowPosition = 0)` / `WriteRow(..., IEnumerable<object> data, ...)`

Writes a row to a range.

---

#### `void WriteColumn(ISpreadsheet spreadsheet, IRange range, DataColumn data, RangeWriteMode writeMode = RangeWriteMode.AppendRight, int overwriteColumnIndex = 0)` / `WriteColumn(..., IEnumerable<object> data, ...)`

Writes a column to a range.

---

### Deleting Data

#### `void DeleteRange(ISpreadsheet spreadsheet, IRange range, RangeDeleteMode deleteMode = RangeDeleteMode.Rows)`

Deletes data in a range and shifts remaining cells. See [`RangeDeleteMode`](#rangedeletemode).

---

#### `void DeleteRows(ISpreadsheet spreadsheet, IRange range, string rows, RowsDeleteMode deleteMode = RowsDeleteMode.Delete)`

Deletes rows by index string (e.g., `"0, 2, 5-7"`). See [`RowsDeleteMode`](#rowsdeletemode).

---

#### `void DeleteRows(ISpreadsheet spreadsheet, IRange range, IEnumerable<int> rowIndices, RowsDeleteMode deleteMode = RowsDeleteMode.Delete)`

Deletes rows by index collection.

---

#### `void DeleteColumn(ISpreadsheet spreadsheet, IRange range, string columnName, bool hasHeaders = true, ColumnDeleteMode deleteMode = ColumnDeleteMode.Delete)` / `DeleteColumn(..., int columnIndex, ...)`

Deletes a column by name or index. See [`ColumnDeleteMode`](#columndeletemode).

Both overloads have variants that also return `out IRangeInformation rangeInformation`.

---

### Formatting and Utilities

#### `Color GetCellColor(ISpreadsheet spreadsheet, IRange range)`

Gets the background color of the first cell in a range.

**Returns:** `System.Drawing.Color`.

---

#### `void SetRangeColor(ISpreadsheet spreadsheet, IRange range, Color color)`

Sets the background color for all cells in a range.

---

#### `void AutoFillRange(ISpreadsheet spreadsheet, IRange range, int fillLength, FillDirection direction)`

Extends a pattern or formula from the source range. See [`FillDirection`](#filldirection).

---

#### `void CopyPasteRange(ISpreadsheet spreadsheet, IRange source, IRange destination, SheetPasteType pasteType = SheetPasteType.Normal, SheetPasteOrientation pasteOrientation = SheetPasteOrientation.Normal)`

Copies data from one range and pastes it to another. See [`SheetPasteType`](#sheetpastetype) and [`SheetPasteOrientation`](#sheetpasteorientation).

---

## `IGoogleDocsService` — Google Docs

Obtained via `google.Docs(new DocsConnection("connId"))`.

### Methods

#### `IReadOnlyCollection<IDocument> GetDocuments(GDriveRemoteItem parentFolder = null)`

Lists documents in a Drive folder.

**Returns:** Collection of `IDocument`.

---

#### `IDocument AddDocument(GDriveRemoteItem parentFolder, string documentName, ConflictBehavior conflictResolution = ConflictBehavior.Fail)`

Creates a new Google Docs document.

**Returns:** The new `IDocument`.

---

#### `IDocumentInfo GetDocument(string documentId)`

Retrieves document metadata by ID or URL.

**Returns:** `IDocumentInfo` — document metadata (Id, Name, underlying model).

---

#### `string ReadText(IDocument document, string sectionName = null, bool matchCase = false, TextMatchMode textMatchMode = TextMatchMode.Contains)`

Reads text from a document or a specific section.

**Parameters:**
- `sectionName` — Section heading to read from. `null` reads the entire document.
- `textMatchMode` — How to match the section name. See [`TextMatchMode`](#textmatchmode).

**Returns:** The text content as `string`.

---

#### `void WriteText(IDocument document, string text, TextStyle style = TextStyle.NormalText, TextLocation location = TextLocation.Beginning, string sectionName = null, bool matchCase = false, TextMatchMode textMatchMode = TextMatchMode.Contains)`

Writes or appends text to a document or section. See [`TextStyle`](#textstyle) and [`TextLocation`](#textlocation).

---

#### `void DeleteText(IDocument document, string text, bool matchCase = false, TextRecurrences recurrences = TextRecurrences.Once)`

Deletes occurrences of specific text. See [`TextRecurrences`](#textrecurrences).

---

#### `void DeleteText(IDocument document, string sectionName, bool matchCase = false, TextMatchMode textMatchMode = TextMatchMode.Contains)`

Deletes an entire section identified by its heading name.

---

#### `void FillDocumentTemplate(IDocument document, IReadOnlyDictionary<string, string> fields, string symbol = "{{ }}")`

Replaces template placeholders (e.g., `{{FieldName}}`) with values.

**Parameters:**
- `fields` — Dictionary of field name to replacement value.
- `symbol` — Delimiter used around field names. Default: `"{{ }}"`.

---

#### `void FindAndReplaceText(IDocument document, string searchTerm, string replacement, bool matchCase = false, TextRecurrences recurrences = TextRecurrences.Once)`

Finds and replaces text in a document.

---

#### `void InsertImage(IDocument document, IFile imageFile, TextLocation location = TextLocation.Beginning, string sectionName = null, bool matchCase = false, TextMatchMode textMatchMode = TextMatchMode.Contains)`

Inserts an image from Google Drive into a document.

---

## `IGoogleTasksService` — Google Tasks

Obtained via `google.Tasks(new TasksConnection("connId"))`.

### Methods

#### `IReadOnlyCollection<ITaskList> GetTaskLists()`

Retrieves all task lists for the authenticated account.

**Returns:** Collection of `ITaskList`.

---

#### `ITaskList GetTaskList(string id)`

Retrieves a task list by ID.

**Returns:** `ITaskList`.

---

#### `ITaskList CreateTaskList(string name)`

Creates a new task list.

**Returns:** The new `ITaskList`.

---

#### `ITaskList RenameTaskList(ITaskList list, string newName)`

Renames a task list.

**Returns:** The updated `ITaskList`.

---

#### `void DeleteTaskList(ITaskList list)`

Deletes a task list and all its tasks.

---

#### `IReadOnlyCollection<ITask> GetTasks(ITaskList list, ITask parentTask = null, bool includeCompleted = false, int maxResults = 100)`

Retrieves tasks from a task list.

**Parameters:**
- `parentTask` — When specified, returns only subtasks of this task.
- `includeCompleted` — Whether to include completed tasks.

**Returns:** Collection of `ITask`.

---

#### `ITask GetTask(string taskId, ITaskList list)`

Retrieves a specific task by ID.

**Returns:** `ITask`.

---

#### `ITask CreateTask(ITaskList list, string title, string description = null, DateTime? dueDate = null, GTaskStatus status = GTaskStatus.NeedsAction, string parentTaskId = null)`

Creates a new task. Set `parentTaskId` to create a subtask.

**Returns:** The new `ITask`.

---

#### `ITask UpdateTask(ITask task, string newTitle = null, string newDescription = null, DateTime? newDueDate = null, GTaskStatus? newStatus = null)`

Updates an existing task. `null` parameters leave the current values unchanged.

**Returns:** The updated `ITask`.

---

#### `void DeleteTask(ITask task)`

Deletes a task.

---

#### `ITask CompleteTask(ITask task)`

Marks a task as completed.

**Returns:** The updated `ITask` with `Completed` status.

---

## `IFormsService` — Google Forms

Obtained via `google.Forms(new FormsConnection("connId"))`.

### Methods

#### `IForm GetFormInfo(string formId)`

Retrieves metadata and structure of a Google Form.

**Returns:** `IForm` with `FormId`, `Title`, and the underlying `GFormInfo` model.

---

#### `IReadOnlyCollection<IFormResponse> GetFormResponses(string formId, DateTime? submittedAfter = null)`

Retrieves responses submitted to a form.

**Parameters:**
- `submittedAfter` — Filter to return only responses after this timestamp.

**Returns:** Collection of `IFormResponse`.

---

## `IGoogleWorkspaceService` — Workspace (HTTP / Apps Script)

Obtained via `google.Workspace(new WorkspaceConnection("connId"))`.

### Methods

#### `HttpRequestResult HttpRequest(string url, RequestMethod method = RequestMethod.GET, IDictionary<string, string> headers = null, IDictionary<string, string> parameters = null, string body = null)`

Sends an HTTP request authenticated with the Google OAuth credential.

**Returns:** `HttpRequestResult` with `StatusCode`, `Content`, `Headers`, and `IsSuccess`.

---

#### `IDictionary<string, object> RunScript(string scriptId, string function, IEnumerable<object> parameters = null, bool devMode = false)`

Executes a Google Apps Script function.

**Parameters:**
- `scriptId` — Deployment ID (or script ID for dev mode).
- `function` — Name of the function to execute.
- `devMode` — Run with the script ID in development mode.

**Returns:** Dictionary of result values.

---

## Handle Types

Handle types are returned by service methods and expose additional operations on the underlying Google Workspace resource.

---

### `IMail`

Represents a Gmail message.

**Properties:**
- `GMailItem Item` — The underlying Gmail model.
- `string Id` — Message ID.
- `string Subject` — Message subject.
- `string Body` — Message body.

**Methods:**
- `void Reply(string body, ...)` / `Reply(ReplyToEmailRequest options)` — Reply to this message.
- `void Forward(string body, ...)` / `Forward(ForwardEmailRequest options)` — Forward this message.
- `void Move(IMailLabel source, IMailLabel destination)` — Move to a different label.
- `void Archive()` — Remove from Inbox.
- `void Delete(bool deletePermanently = false)` — Delete (to Trash or permanently).
- `Stream Download()` — Download as EML stream.
- `IReadOnlyCollection<IMailAttachmentInfo> GetAttachmentsInfo()` — Get attachment metadata.
- `IReadOnlyDictionary<IMailAttachmentInfo, Stream> DownloadAttachments()` — Download all attachments.
- `void MarkAsRead()` — Mark as read.
- `void MarkAsUnread()` — Mark as unread.
- `void ApplyLabels(IReadOnlyList<IMailLabel> labels)` — Apply labels.
- `void RemoveLabels(IReadOnlyList<IMailLabel> labels)` — Remove labels.

---

### `IMailLabel`

Represents a Gmail label (system or user-defined).

**Properties:**
- `string Id` — Label ID.
- `string Name` — Label display name.
- `bool IsUserDefined` — Whether this is a user-created label.

**Methods:**
- `IMail GetNewestEmail(MailFilter filter = null, bool markAsRead = false)` — Get the newest email in this label.
- `IReadOnlyCollection<IMail> GetEmails(MailFilter filter = null, bool markAsRead = false, int? maxResults = null)` — Get emails in this label.
- `void ApplyTo(IMail mail)` — Apply this label to an email.
- `void RemoveFrom(IMail mail)` — Remove this label from an email.

---

### `IMailAttachmentInfo`

Metadata for a Gmail attachment.

**Properties:**
- `string Name` — Attachment file name.
- `string Id` — Attachment ID.

**Methods:**
- `Stream Download()` — Downloads the attachment content.

---

### `MailSystemFolders`

Provides access to Gmail's built-in system label folders.

**Properties (all `IMailLabel`):**
- `AllMail` — All messages in the account.
- `Inbox` — The Inbox folder.
- `Important` — Messages marked as important.
- `Unread` — Unread messages.
- `Sent` — Sent messages.
- `Draft` — Draft messages.
- `Spam` — Spam folder.
- `Starred` — Starred messages.
- `Trash` — Trash folder.

---

### `ICalendar`

Represents a Google Calendar.

**Properties:**
- `GSuiteCalendarItem Item` — The underlying calendar model.
- `string Id` — Calendar ID.
- `bool IsDefault` — Whether this is the user's primary calendar.

**Methods:**
- `IReadOnlyCollection<ICalendarEvent> GetEvents(DateTimeOffset startDate, DateTimeOffset endDate, int top = 50, string timezone = "", string search = "")` — List events.
- `ICalendarEvent CreateEvent(CreateCalendarItem eventInformation)` — Create an event.
- `ICalendarEvent UpdateEvent(ICalendarEvent calendarEvent, UpdateCalendarItem changes)` — Update an event.
- `void DeleteEvent(ICalendarEvent calendarEvent, DeleteEventMode deleteMode = DeleteEventMode.SingleEvent)` — Delete an event.
- `void ForwardEvent(ICalendarEvent calendarEvent, IEnumerable<string> attendees, bool forwardSeries = false)` — Forward an event.
- `void RespondToEvent(ICalendarEvent calendarEvent, EventResponseType response, ...)` — RSVP to an event.

---

### `ICalendarEvent`

Represents a Google Calendar event.

**Properties:**
- `ICalendar Calendar` — The calendar this event belongs to.
- `GSuiteEventItem Item` — The underlying event model (includes attendees, start/end times, etc.).
- `string Id` — Event ID.

**Methods:**
- `void Delete(DeleteEventMode deleteMode = DeleteEventMode.SingleEvent)` — Cancel this event.
- `void Forward(IEnumerable<string> attendees, bool forwardSeries = false)` — Forward to additional attendees.
- `void Respond(EventResponseType response, string comment = "", int? additionalGuests = null, bool allOccurrences = false, bool sendResponseNotification = false)` — RSVP.
- `ICalendarEvent Update(UpdateCalendarItem changes)` — Update event details.

---

### `IDriveItem`

Base type for files and folders in Google Drive.

**Properties:**
- `GDriveRemoteItem Item` — The underlying Drive model (usable as input to Sheets/Docs service methods).
- `string Id` — Drive item ID.
- `bool IsFolder` — Whether this item is a folder.
- `string FullName` — Full path name.

**Methods:**
- `IDriveItem Refresh()` — Get the latest version.
- `IDriveItem Move(IFolder destination, ...)` — Move to a different folder.
- `void Delete()` — Delete this item.
- `string Share(Role role = Role.Reader, ...)` — Share with anyone.
- `string Share(string domain, Role role = Role.Reader, ...)` — Share with a domain.
- `string Share(GranteeType shareWith, string recipients, ...)` — Share with users/groups.

---

### `IFile`

Extends `IDriveItem`. Represents a file in Google Drive.

**Additional Methods:**
- `IFile Copy(IFolder destination, ...)` — Copy this file.
- `Stream Download(DownloadOptions options = null)` — Download this file.
- `IFile Upload(...)` — Upload new content to replace this file.
- `IFile Move(IFolder destination, ...)` — Move this file.
- `IReadOnlyCollection<ILabel> GetLabels()` — Get Drive labels applied to this file.
- `void RemoveLabels(IEnumerable<ILabel> labels)` — Remove labels.
- `void ClearLabelFields(string labelId, IEnumerable<GDriveLabelField> fields)` — Clear label field values.
- `void ApplyLabels(IEnumerable<ILabel> labels)` — Apply labels.

---

### `IFolder`

Extends `IDriveItem`. Represents a folder in Google Drive.

**Additional Methods:**
- `IFolder Move(IFolder destination, ...)` — Move this folder.
- `IDriveItem GetItem(string relativePath)` — Get an item by relative path.
- `IFile GetFile(string relativePath)` — Get a file by relative path.
- `IFolder GetFolder(string relativePath)` — Get a subfolder by relative path.
- `IReadOnlyCollection<IDriveItem> GetFilesAndFolders(...)` — List contents.
- `IReadOnlyCollection<IFolder> GetFolders(...)` — List subfolders.
- `IReadOnlyCollection<IFile> GetFiles(...)` — List files.
- `IFolder CreateFolder(string name, ...)` — Create a subfolder.

---

### `IDriveItemInfo`

Rich metadata for a Drive file or folder.

**Properties:**
- `string Id` — Item ID.
- `string Name` — Item name.
- `GDriveItemInfo Item` — Full metadata model including MIME type, size, owners, etc.

---

### `ILabel`

Represents a Drive label with configurable field values.

**Properties:**
- `GDriveLabel LabelItem` — The underlying label model.
- `string Id` — Label ID.
- `string Name` — Label name.
- `GDriveLabelFieldCollection Fields` — Available field definitions.
- `Dictionary<string, object> FieldValues` — Current field values (get/set).
- `object this[string fieldName]` — Indexer to get/set a field by name.

**Methods (fluent builders — return `ILabel` for chaining):**
- `ILabel UpdateTextFieldValue(string fieldName, string value)`
- `ILabel UpdateNumberFieldValue(string fieldName, long value)`
- `ILabel UpdateDateFieldValue(string fieldName, DateTime value)`
- `ILabel UpdateChoiceFieldValue(string fieldName, List<string> value)`
- `ILabel UpdateUserFieldValue(string fieldName, string value)`
- `void ApplyToFile(IFile file)` — Apply this label to a file.

---

### `ISpreadsheet`

Represents a Google Sheets spreadsheet.

**Properties:**
- `GDriveRemoteItem Item` — The underlying Drive item (use this as `parentFolder` in Docs/Sheets service calls).
- `string Id` — Spreadsheet ID.
- `string Name` — Spreadsheet name.

**Methods:**
- `ISheet AddSheet(string sheetName = null, int? positionIndex = null)` — Add a sheet tab.
- `void RenameSheet(string oldName, string newName)` — Rename a sheet tab.
- `void DeleteSheet(string sheetName)` — Delete a sheet tab.
- `IReadOnlyCollection<ISheet> GetSheets()` — List all sheets.
- `IReadOnlyCollection<IRange> GetRanges()` — List all sheets and named ranges.
- `IReadOnlyCollection<INamedRange> GetNamedRanges()` — List named ranges only.

---

### `IRange`

Base type for sheets and named ranges. Supports direct range operations.

**Properties:**
- `string Name` — Range or sheet name.
- `RangeType Type` — Whether this is a `Sheet`, `NamedRange`, or `Any`.

**Methods:**
- `DataTable ReadRange(bool hasHeaders = true, CellReadMode cellReadMode = CellReadMode.Values)` — Read into DataTable.
- `void WriteRange(DataTable data, bool hasHeaders = true, RangeWriteMode writeMode = RangeWriteMode.Overwrite, int insertRowPosition = 0)` — Write DataTable.
- `void WriteRow(DataRow data, ...)` / `WriteRow(IEnumerable<object> data, ...)` — Write a row.
- `void WriteColumn(DataColumn data, ...)` / `WriteColumn(IEnumerable<object> data, ...)` — Write a column.
- `void DeleteRange(RangeDeleteMode deleteMode)` — Delete range data.
- `void DeleteRows(string rows, RowsDeleteMode deleteMode)` / `DeleteRows(IEnumerable<int> rowsIndexes, ...)` — Delete rows.
- `void DeleteColumn(string columnName, ...)` / `DeleteColumn(int columnIndex, ...)` — Delete a column.

---

### `ISheet`

Extends `IRange`. Represents a sheet (tab) within a spreadsheet.

**Additional Methods:**
- `void Delete()` — Delete this sheet.
- `void Rename(string newName)` — Rename this sheet.
- `object ReadCell(string cell, CellReadMode cellReadMode = CellReadMode.Values)` — Read a single cell (e.g., `"A1"`).
- `void WriteCell(string cell, object value)` — Write a single cell.

---

### `INamedRange`

Extends `IRange`. Represents a named range within a spreadsheet. No additional members beyond `IRange`.

---

### `IDocument`

Represents a Google Docs document.

**Properties:**
- `GDriveRemoteItem Item` — The Drive item for this document.
- `string Id` — Document ID.
- `string Name` — Document name.

**Methods:**
- `string ReadText(string sectionName = null, bool matchCase = false, TextMatchMode textMatchMode = TextMatchMode.Contains)` — Read text content.
- `void WriteText(string text, TextStyle style = TextStyle.NormalText, TextLocation location = TextLocation.Beginning, ...)` — Write or append text.
- `void DeleteText(string text, bool matchCase = false, TextRecurrences recurrences = TextRecurrences.Once)` — Delete text occurrences.
- `void DeleteText(string sectionName, bool matchCase = false, TextMatchMode textMatchMode = TextMatchMode.Contains)` — Delete a section.
- `void FillDocumentTemplate(IReadOnlyDictionary<string, string> fields, string symbol = "{{ }}")` — Fill template fields.
- `void FindAndReplaceText(string searchTerm, string replacement, bool matchCase = false, TextRecurrences recurrences = TextRecurrences.Once)` — Find and replace.

---

### `ITask`

Represents a Google Task.

**Properties:**
- `GTaskItem Item` — The underlying task model.
- `string Id` — Task ID.
- `string Title` — Task title.
- `string Details` — Task description.
- `GTaskStatus Status` — Task status (`NeedsAction` or `Completed`).
- `DateTime? Due` — Due date.
- `string ParentListId` — ID of the containing task list.

**Methods:**
- `ITask Update(string newTitle = null, string newDescription = null, DateTime? newDueDate = null, GTaskStatus? newStatus = null)` — Update this task.
- `void Delete()` — Delete this task.
- `ITask Complete()` — Mark as completed.
- `IReadOnlyCollection<ITask> GetTasks(bool includeCompleted = false, int maxResults = 100)` — Get subtasks.
- `ITask CreateTask(string title, string description = null, DateTime? dueDate = null, GTaskStatus status = GTaskStatus.NeedsAction)` — Create a subtask.

---

### `ITaskList`

Represents a Google Tasks task list.

**Properties:**
- `GTaskListItem Item` — The underlying model.
- `string Id` — Task list ID.
- `string Title` — Task list name.

**Methods:**
- `IReadOnlyCollection<ITask> GetTasks(ITask parentTask = null, bool includeCompleted = false, int maxResults = 100)` — List tasks.
- `ITask GetTask(string taskId)` — Get a task by ID.
- `ITask CreateTask(string title, string description = null, DateTime? dueDate = null, GTaskStatus status = GTaskStatus.NeedsAction)` — Create a task.
- `void Delete()` — Delete this task list.
- `ITaskList Rename(string newName)` — Rename this task list.

---

### `IForm`

Represents a Google Form.

**Properties:**
- `GFormInfo Item` — The underlying form model with full structure.
- `string FormId` — Form ID.
- `string Title` — Form title.

**Methods:**
- `IReadOnlyCollection<IFormResponse> GetResponses(DateTime? submittedAfter = null)` — Retrieve form responses.

---

### `IFormResponse`

Represents a submitted Google Form response.

**Properties:**
- `GFormResponse Item` — The underlying response model with all answer data.
- `string FormId` — ID of the form.
- `string ResponseId` — Unique ID of this response.

---

## Builder Classes

### `SendEmailRequest`

Fluent builder for composing emails to send.

```csharp
var request = new SendEmailRequest()
    .WithTo("recipient@example.com")
    .WithSubject("Hello")
    .WithHtmlBody("<p>Hello World</p>")
    .WithCc("cc@example.com")
    .WithAttachment("/path/to/file.pdf")
    .WithImportance(GMailImportance.High);
```

**Builder Methods:**
- `.WithTo(string email)` / `.WithTo(IEnumerable<string> emails)` — Add primary recipients.
- `.WithCc(string email)` / `.WithCc(IEnumerable<string> emails)` — Add CC recipients.
- `.WithBcc(string email)` / `.WithBcc(IEnumerable<string> emails)` — Add BCC recipients.
- `.WithSubject(string subject)` — Set subject.
- `.WithBody(string body)` — Set plain-text body.
- `.WithHtmlBody(string html)` — Set HTML body.
- `.WithAttachment(string filePath)` / `.WithAttachment(Stream stream, string fileName)` — Add attachment.
- `.WithReplyTo(string email)` — Set reply-to address.
- `.WithImportance(GMailImportance importance)` — Set importance (`Low`, `Normal`, `High`).
- `.AsDraft()` — Save as draft instead of sending.

---

### `ReplyToEmailRequest`

Extends `SendEmailRequest` with reply-specific options.

**Additional Builder Methods:**
- `.WithNewSubject(string subject)` — Override the reply subject.
- `.WithReplyToAll()` — Reply to all original recipients.
- `.WithImportance(GMailImportance importance)` — Set importance.

---

### `ForwardEmailRequest`

Extends `SendEmailRequest` with forward-specific options.

**Additional Builder Method:**
- `.WithNewSubject(string subject)` — Override the forwarded subject.

---

### `CreateCalendarItem`

Fluent builder for creating a new calendar event.

```csharp
var newEvent = new CreateCalendarItem()
    .WithTitle("Team Meeting")
    .WithStartDate(DateTimeOffset.UtcNow.AddHours(1))
    .WithEndDate(DateTimeOffset.UtcNow.AddHours(2))
    .WithTimezone("America/New_York")
    .AddRequiredAttendees("alice@example.com", "bob@example.com")
    .NewDescription("Weekly sync")
    .WithConferenceData(true);
```

**Builder Methods:**
- `.WithTitle(string title)`
- `.WithStartDate(DateTimeOffset startDate)`
- `.WithEndDate(DateTimeOffset endDate)`
- `.WithTimezone(string timezone)` — Default: `"UTC"`.
- `.AllDayEvent(bool isAllDay)`
- `.AddRequiredAttendees(params string[] emails)` / `.AddRequiredAttendees(IEnumerable<string> emails)`
- `.AddOptionalAttendees(params string[] emails)` / `.AddOptionalAttendees(IEnumerable<string> emails)`
- `.AddResourceAttendees(params string[] emails)` / `.AddResourceAttendees(IEnumerable<string> emails)`
- `.NewDescription(string description)`
- `.WithConferenceData(bool addConferenceData)` — Add Google Meet link.
- `.ShowAs(EventTransparency transparency)` — `Opaque` (blocks time) or `Transparent`.
- `.WithVisibility(EventVisibility visibility)` — `DEFAULT`, `PUBLIC`, `PRIVATE`, or `CONFIDENTIAL`.
- `.WithPreferredReturnTimezone(string timezone)`
- `.SendNotification(SendUpdates updates)` — `ALL`, `EXTERNAL_ONLY`, or `NONE`.
- `.CanModifyEvent(bool canModify)`
- `.CanInviteOthers(bool canInviteOthers)`
- `.CanSeeAttendeesList(bool canSeeAttendeesList)`

---

### `UpdateCalendarItem`

Fluent builder for modifying an existing calendar event. Initialized with the current event's attendees.

```csharp
var changes = new UpdateCalendarItem(existingEvent)
    .WithTitle("Updated Meeting Title")
    .AddRequiredAttendees("newperson@example.com")
    .RemoveOptionalAttendees("oldperson@example.com");
```

**Builder Methods:** Same as `CreateCalendarItem`, plus:
- `.ClearRequiredAttendees()` / `.OverwriteRequiredAttendees(...)` / `.RemoveRequiredAttendees(...)`
- `.ClearOptionalAttendees()` / `.OverwriteOptionalAttendees(...)` / `.RemoveOptionalAttendees(...)`
- `.ClearResourceAttendees()` / `.OverwriteResourceAttendees(...)` / `.RemoveResourceAttendees(...)`

---

## Options Classes

### `DownloadOptions`

Specifies export formats when downloading Google Workspace native file types.

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `DocumentExportFormat` | `GDocExportFormat` | `Word` | Format for Google Docs: `Word`, `OpenDocument`, `RichText`, `Pdf`, `PlainText`, `WebPage`, `EPub`. |
| `DrawingExportFormat` | `GDrawingExportFormat` | `Jpeg` | Format for Google Drawings: `Jpeg`, `Png`, `Svg`, `Pdf`, `Odp`. |
| `SpreadsheetExportFormat` | `GSheetExportFormat` | `Xlsx` | Format for Google Sheets: `Xlsx`, `Ods`, `Pdf`, `WebPage`, `Csv`, `Tsv`. |
| `PresentationExportFormat` | `GSlideExportFormat` | `Ppt` | Format for Google Slides: `Ppt`, `Odp`, `Pdf`, `PlainText`, `Jpeg`, `Png`, `Svg`. |

---

### `MailFilter`

Fluent builder for filtering Gmail messages. Use `DriveItemFilter` for Drive filtering.

```csharp
var filter = new MailFilter()
    .WithFrom("sender@example.com")
    .WithSubject("Invoice")
    .WithDateAndTime(DateTimeOffset.UtcNow.AddDays(-7), null);
```

**Builder Methods:**
- `.WithFrom(string email)`
- `.WithTo(string email)`
- `.WithCc(string email)`
- `.WithBcc(string email)`
- `.WithSubject(string subject)`
- `.WithBody(string text)`
- `.WithFilename(string filename)` — Filter by attachment file name.
- `.WithDateAndTime(DateTimeOffset? after, DateTimeOffset? before)`
- `.WithLabels(IEnumerable<IMailLabel> labels)`

---

### `DriveItemFilter`

Fluent builder for filtering Drive items.

**Builder Methods:**
- `.WithName(string name)`
- `.WithOwner(string ownerEmail)`
- `.WithCreationDateTime(DateTimeOffset? after, DateTimeOffset? before)`
- `.WithLastModifiedDateTime(DateTimeOffset? after, DateTimeOffset? before)`
- `.WithTextInFile(string text)` — Full-text search within file content.
- `.WithType(FileTypes type)` — Filter by file type.

---

### `HttpRequestResult`

Returned by `IGoogleWorkspaceService.HttpRequest()`.

| Property | Type | Description |
|----------|------|-------------|
| `Content` | `string` | Response body content. |
| `StatusCode` | `int` | HTTP status code. |
| `Headers` | `IDictionary<string, string>` | Response headers. |
| `IsSuccess` | `bool` | `true` when `StatusCode` is 200–299. |

---

## Enums

### `DeleteEventMode`

How to delete recurring calendar events.

| Value | Description |
|-------|-------------|
| `SingleEvent` | Delete only the selected occurrence. |
| `FutureOnly` | Delete this and all future occurrences. |
| `PastAndFuture` | Delete all occurrences (past and future). |

---

### `EventResponseType`

RSVP response for calendar event invitations.

| Value | Description |
|-------|-------------|
| `Declined` | The attendee has declined. |
| `Tentative` | The attendee has tentatively accepted. |
| `Accepted` | The attendee has accepted. |

---

### `GTaskStatus`

Task completion status.

| Value | Description |
|-------|-------------|
| `NeedsAction` | Task is not yet completed (default). |
| `Completed` | Task has been completed. |

---

### `GMailImportance`

Email importance level for `SendEmailRequest`.

| Value | Description |
|-------|-------------|
| `Low` | Low importance. |
| `Normal` | Normal importance (default). |
| `High` | High importance. |

---

### `CellReadMode`

How cell values are returned when reading from Sheets.

| Value | Description |
|-------|-------------|
| `Values` | Computed values (default). |
| `Formulas` | Raw formula strings (e.g., `=SUM(A1:A5)`). |
| `Text` | Formatted display text. |

---

### `RangeWriteMode`

How data is written to a range in Sheets.

| Value | Description |
|-------|-------------|
| `Overwrite` | Replace existing data (default for `WriteRange`). |
| `Append` | Append rows below existing data. |
| `AppendRight` | Append columns to the right. |
| `Insert` | Insert rows at a specific position. |
| `InsertRight` | Insert columns at a specific position. |

---

### `RangeDeleteMode`

What to shift after deleting a range in Sheets.

| Value | Description |
|-------|-------------|
| `None` | Clear cell content only. |
| `Rows` | Delete rows and shift remaining rows up. |
| `Columns` | Delete columns and shift remaining columns left. |

---

### `RowsDeleteMode`

How rows are deleted in Sheets.

| Value | Description |
|-------|-------------|
| `Clear` | Clear row content but keep the row structure. |
| `Delete` | Remove rows and shift remaining rows up. |

---

### `ColumnDeleteMode`

How columns are deleted in Sheets.

| Value | Description |
|-------|-------------|
| `Clear` | Clear column content but keep the column structure. |
| `Delete` | Remove column and shift remaining columns left. |

---

### `FillDirection`

Direction to auto-fill in Sheets.

| Value | Description |
|-------|-------------|
| `Up` | Fill cells above the source range. |
| `Down` | Fill cells below the source range. |
| `Left` | Fill cells to the left of the source range. |
| `Right` | Fill cells to the right of the source range. |

---

### `SheetPasteType`

What to paste in a copy-paste operation.

| Value | Description |
|-------|-------------|
| `Normal` | Paste all content and formatting (default). |
| `Values` | Paste values only (no formulas or formatting). |
| `Format` | Paste formatting only. |
| `NoBorders` | Paste all except borders. |
| `Formula` | Paste formulas only. |
| `DataValidation` | Paste data validation rules only. |
| `ConditionalFormatting` | Paste conditional formatting only. |

---

### `SheetPasteOrientation`

Transpose option for copy-paste in Sheets.

| Value | Description |
|-------|-------------|
| `Normal` | Paste in the same orientation (default). |
| `Transpose` | Transpose rows and columns when pasting. |

---

### `TextStyle`

Heading styles for writing to Google Docs.

`NormalText`, `Title`, `Subtitle`, `Heading1`, `Heading2`, `Heading3`, `Heading4`, `Heading5`, `Heading6`

---

### `TextLocation`

Where to insert text relative to a section or document.

| Value | Description |
|-------|-------------|
| `Beginning` | Insert at the start. |
| `End` | Insert at the end. |

---

### `TextMatchMode`

How section names are matched in Docs.

| Value | Description |
|-------|-------------|
| `Contains` | Section name contains the search string. |
| `Equals` | Section name exactly matches the search string. |

---

### `TextRecurrences`

How many text occurrences to process in Docs.

| Value | Description |
|-------|-------------|
| `Once` | Process only the first occurrence. |
| `AllRecurrences` | Process all occurrences. |

---

### `ConflictBehavior`

How to handle name conflicts when creating or copying Drive items.

| Value | Description |
|-------|-------------|
| `Replace` | Overwrite the existing item. |
| `Fail` | Throw an error if a conflict exists. |
| `Rename` | Rename the new item to be unique. |
| `AddSeparate` | Add the new item without renaming, even if a duplicate exists. |

---

### `RenameConflictBehavior`

How to handle name conflicts when renaming Drive items.

| Value | Description |
|-------|-------------|
| `Fail` | Throw an error if a conflict exists. |
| `AddSeparate` | Rename anyway, even if a duplicate name exists. |

---

### `Role`

Permission role for Drive sharing.

| Value | Description |
|-------|-------------|
| `Owner` | Full ownership. |
| `Writer` | Can edit. |
| `Commenter` | Can comment only. |
| `Reader` | Can view only. |

---

### `GranteeType`

Recipient type for Drive sharing.

| Value | Description |
|-------|-------------|
| `User` | Share with a specific user email. |
| `Group` | Share with a Google Group. |

---

### `IdentificationType`

How a Drive item is identified.

| Value | Description |
|-------|-------------|
| `UrlOrId` | Identifier is a URL or Drive item ID (default). |
| `FullPath` | Identifier is a full folder path. |

---

### `DriveLabelType`

Which Drive labels to retrieve.

| Value | Description |
|-------|-------------|
| `Badged` | Retrieve only badged labels. |
| `Standard` | Retrieve only standard labels. |
| `All` | Retrieve all label types (default). |

---

### `RangeType`

Type of a Sheets range object.

| Value | Description |
|-------|-------------|
| `Sheet` | The range is a sheet (tab). |
| `NamedRange` | The range is a named range. |
| `Any` | Either type. |

---

## Common Patterns

### 1 — Send an Email with HTML Body and Attachment

```csharp
[Workflow]
public void Execute()
{
    var gmail = google.Gmail(new GmailConnection("myGmailConn"));

    gmail.SendEmail(new SendEmailRequest()
        .WithTo("manager@example.com")
        .WithSubject("Monthly Report")
        .WithHtmlBody("<h1>Report Ready</h1><p>Please find the report attached.</p>")
        .WithAttachment(@"C:\Reports\report.pdf")
        .WithImportance(GMailImportance.High));
}
```

---

### 2 — Read Emails and Process Attachments

```csharp
[Workflow]
public void Execute()
{
    var gmail = google.Gmail(new GmailConnection("myGmailConn"));

    var filter = new MailFilter()
        .WithSubject("Invoice")
        .WithDateAndTime(DateTimeOffset.UtcNow.AddDays(-7), null);

    var emails = gmail.GetEmails(
        folder: gmail.SystemFolders.Inbox,
        filter: filter,
        markAsRead: true,
        maxResults: 20);

    foreach (var email in emails)
    {
        var attachments = email.GetAttachmentsInfo();
        foreach (var att in attachments)
        {
            using var stream = att.Download();
            // process stream...
            Log($"Downloaded: {att.Name}");
        }
    }
}
```

---

### 3 — Create and Manage Google Sheets Data

```csharp
[Workflow]
public void Execute()
{
    var sheets = google.Sheets(new SheetsConnection("mySheetsConn"));

    // Get a spreadsheet by its Drive ID
    var spreadsheet = sheets.GetSpreadsheets()
        .First(s => s.Name == "Sales Data");

    // Get the first sheet
    var sheet = sheets.GetSheets(spreadsheet).First();

    // Read all data
    var data = sheets.ReadRange(spreadsheet, sheet);
    Log($"Rows: {data.Rows.Count}");

    // Write a new row
    sheets.WriteRow(spreadsheet, sheet,
        new object[] { DateTime.Today.ToString("yyyy-MM-dd"), "Product A", 500 },
        hasHeaders: true,
        writeMode: RangeWriteMode.Append);

    // Set header row background to blue
    sheets.SetRangeColor(spreadsheet, sheet, System.Drawing.Color.SteelBlue);
}
```

---

### 4 — Manage Calendar Events

```csharp
[Workflow]
public void Execute()
{
    var calendar = google.Calendar(new GmailConnection("myGmailConn"));

    var defaultCal = calendar.GetDefaultCalendar();

    // Create an event
    var newEvent = calendar.CreateEvent(defaultCal,
        new CreateCalendarItem()
            .WithTitle("Project Review")
            .WithStartDate(DateTimeOffset.UtcNow.AddDays(1))
            .WithEndDate(DateTimeOffset.UtcNow.AddDays(1).AddHours(1))
            .WithTimezone("Europe/London")
            .AddRequiredAttendees("alice@example.com", "bob@example.com")
            .WithConferenceData(true)
            .SendNotification(SendUpdates.ALL));

    Log($"Event created: {newEvent.Id}");

    // Get upcoming events
    var events = calendar.GetEvents(
        startDate: DateTimeOffset.UtcNow,
        endDate: DateTimeOffset.UtcNow.AddDays(7));

    foreach (var ev in events)
    {
        Log($"{ev.Item.Title} — {ev.Item.StartTime}");
    }
}
```

---

### 5 — Upload Files to Drive and Apply Labels

```csharp
[Workflow]
public void Execute()
{
    var drive = google.Drive(new DriveConnection("myDriveConn"));

    // Get destination folder
    var folder = drive.GetFolder("MyProject/Reports");

    // Upload a file
    var uploadedFile = drive.UploadFile(
        path: @"C:\Reports\Q1_2025.xlsx",
        destination: folder,
        conflictBehavior: ConflictBehavior.Replace);

    Log($"Uploaded: {uploadedFile.FullName}");

    // Apply a Drive label
    var labels = drive.GetDriveLabels(DriveLabelType.Standard);
    var confidentialLabel = labels.FirstOrDefault(l => l.Name == "Confidential");

    if (confidentialLabel != null)
    {
        confidentialLabel.UpdateTextFieldValue("Department", "Finance");
        drive.ApplyFileLabels(uploadedFile, new[] { confidentialLabel });
    }

    // Share with a colleague
    var shareUrl = drive.ShareFile(uploadedFile, GranteeType.User,
        recipients: "colleague@example.com",
        role: Role.Reader,
        sendNotificationEmail: true);

    Log($"Share URL: {shareUrl}");
}
```

---

### 6 — Fill a Google Docs Template and Export

```csharp
[Workflow]
public void Execute()
{
    var docs = google.Docs(new DocsConnection("myDocsConn"));
    var drive = google.Drive(new DriveConnection("myDriveConn"));

    // Get the template document
    var templateDoc = docs.GetDocuments()
        .First(d => d.Name == "Contract Template");

    // Fill placeholders — template uses {{FieldName}} syntax
    docs.FillDocumentTemplate(templateDoc,
        new Dictionary<string, string>
        {
            ["ClientName"] = "Acme Corp",
            ["StartDate"] = "2025-04-01",
            ["ContractValue"] = "$50,000"
        });

    // Download as PDF
    var docFile = drive.GetFile(templateDoc.Id);
    using var pdfStream = drive.DownloadFile(docFile,
        new DownloadOptions { DocumentExportFormat = GDocExportFormat.Pdf });

    // Save locally (example)
    using var fileStream = System.IO.File.Create(@"C:\Output\contract.pdf");
    pdfStream.CopyTo(fileStream);

    Log("Contract exported as PDF.");
}
```
