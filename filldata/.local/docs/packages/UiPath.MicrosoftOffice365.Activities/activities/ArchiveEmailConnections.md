# Archive Email

`UiPath.MicrosoftOffice365.Activities.Mail.ArchiveEmailConnections`

Archives an email using Microsoft Office 365 Integration Service.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Mail |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `Email` | Email | Input | [`InArgument<Office365Message>`](types/Office365Message.md) | Yes | | The email to archive. |

## Output Model

This activity does not return an output object.

## XAML Example

```xml
<mail:ArchiveEmailConnections
    DisplayName="Archive Email"
    Email="{x:Null}" />
```

## Notes

- The `Email` property is required.
- The email is moved to the Archive folder.
