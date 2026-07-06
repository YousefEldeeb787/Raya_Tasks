# Delete Email

`UiPath.MicrosoftOffice365.Activities.Mail.DeleteEmailConnections`

Deletes an email using Microsoft Office 365 Integration Service.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Mail |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `Email` | Email | Input | [`InArgument<Office365Message>`](types/Office365Message.md) | Yes | | The email to delete. |
| `PermanentlyDelete` | Permanently Delete | Input | `InArgument<bool>` | No | `False` | Indicates whether to delete the email permanently. |

## Output Model

This activity does not return an output object.

## XAML Example

```xml
<mail:DeleteEmailConnections
    DisplayName="Delete Email"
    Email="{x:Null}"
    PermanentlyDelete="False" />
```

## Notes

- The `Email` property is required.
- When `PermanentlyDelete` is `False`, the email is moved to the Deleted Items folder.
