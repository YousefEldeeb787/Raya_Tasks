# Mark Email As Read/Unread

`UiPath.MicrosoftOffice365.Activities.Mail.MarkAsReadUnreadConnections`

Marks an email as read or unread.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Mail |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `Email` | Email | Input | [`InArgument<Office365Message>`](types/Office365Message.md) | Yes | | The email to mark as read or unread. |
| `MarkAsReadUnread` | Mark As Read/Unread | Input | `InArgument<MarkAsReadUnread>` | No | `Read` | Specifies whether to mark the email as read or unread. |

## Output Model

This activity does not return an output object.

## Enum Reference

| Enum | Values |
|---|---|
| `MarkAsReadUnread` | `Read`, `Unread` |

## XAML Example

```xml
<mail:MarkAsReadUnreadConnections
    DisplayName="Mark As Read"
    Email="{x:Null}"
    MarkAsReadUnread="Read" />
```

## Notes

- The `Email` property is required.
