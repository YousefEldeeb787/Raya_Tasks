# Get Email Thread

`UiPath.MicrosoftOffice365.Activities.Mail.GetEmailThreadConnections`

Retrieves the list of emails within a conversation thread.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Mail |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

| Property | Display Name | Category | Type | Required | Default | Description |
|---|---|---|---|---|---|---|
| `ConversationId` | Conversation ID | Input | `InArgument<string>` | Yes | | The unique identifier of the conversation. |
| `MailboxArg` | Mailbox | | [`MailboxArgument`](components/MailboxArgument.md) | No | | Specifies the mailbox to use. See [MailboxArgument](components/MailboxArgument.md) for input modes. |

## Output Model

The activity returns a `List<`[`Office365Message`](types/Office365Message.md)`>` containing all emails in the thread.

## XAML Example

```xml
<mail:GetEmailThreadConnections
    DisplayName="Get Email Thread"
    ConversationId="{x:Null}" />
```

## Notes

- The `ConversationId` property is required.
- Supports shared mailbox via the `MailboxArg` property.
