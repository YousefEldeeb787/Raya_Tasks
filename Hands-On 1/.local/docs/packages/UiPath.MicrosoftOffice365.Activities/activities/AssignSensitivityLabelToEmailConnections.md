# Assign Sensitivity Label To Email

`UiPath.MicrosoftOffice365.Activities.Mail.AssignSensitivityLabelToEmailConnections`

Assigns a sensitivity label to a specified email.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Mail |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `Item` | Email | Input | [`InArgument<Office365Message>`](types/Office365Message.md) | Yes | | The email to assign the sensitivity label to. |
| `SensitivityLabel` | Sensitivity Label | Input | `InArgument<string>` | Yes | | The sensitivity label to assign. |
| `ActionType` | Action Type | Input | `InArgument<SensitivityLabelActionType>` | No | `Update` | The action type for the sensitivity label operation. |
| `MailboxArg` | Mailbox | | [`MailboxArgument`](components/MailboxArgument.md) | No | | Specifies the mailbox to use. See [MailboxArgument](components/MailboxArgument.md) for input modes. |

## Output Model

This activity does not return an output object.

## Enum Reference

| Enum | Values |
|---|---|
| `SensitivityLabelActionType` | `Update`, `Remove` |

## XAML Example

```xml
<mail:AssignSensitivityLabelToEmailConnections
    DisplayName="Assign Sensitivity Label To Email"
    Item="{x:Null}"
    SensitivityLabel="Confidential" />
```

## Notes

- Both `Item` and `SensitivityLabel` properties are required.
