# Set Email Categories

`UiPath.MicrosoftOffice365.Activities.Mail.SetEmailCategoriesConnections`

Assigns and/or removes categories on an email.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Mail |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `Email` | Email | Input | [`InArgument<Office365Message>`](types/Office365Message.md) | Yes | | The email to associate with the specified categories. |
| `CategoriesToAssign` | Categories To Assign | Input | `InArgument<string[]>` | No | | The categories to add to the email. |
| `CategoriesToRemove` | Categories To Remove | Input | `CategoriesToRemove` | No | `None` | Indicates which categories to remove from the email. |
| `SpecificCategoriesToRemove` | Specific Categories To Remove | Options | `InArgument<string[]>` | No | | The specific categories to remove (when CategoriesToRemove is Specific). |
| `MailboxArg` | Mailbox | | [`MailboxArgument`](components/MailboxArgument.md) | No | | Specifies the mailbox to use. See [MailboxArgument](components/MailboxArgument.md) for input modes. |

## Output Model

This activity does not return an output object.

## Enum Reference

| Enum | Values |
|---|---|
| `CategoriesToRemove` | `None`, `All`, `Specific` |

## XAML Example

```xml
<mail:SetEmailCategoriesConnections
    DisplayName="Set Email Categories"
    Email="{x:Null}"
    CategoriesToRemove="None" />
```

## Notes

- The `Email` property is required.
- If `CategoriesToRemove` is `None`, then `CategoriesToAssign` is required.
- If `CategoriesToRemove` is `Specific`, then `SpecificCategoriesToRemove` is required.
