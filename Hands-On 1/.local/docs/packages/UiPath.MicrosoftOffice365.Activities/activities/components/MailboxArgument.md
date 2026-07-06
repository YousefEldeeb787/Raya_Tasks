# MailboxArgument

**Namespace:** `UiPath.MicrosoftOffice365.Activities.Mail.Models`

A composition argument object used by Outlook mail activities to specify which mailbox to operate on. Uses a boolean toggle (not an enum) to switch between the authenticated user's own mailbox and a shared mailbox.

## InputMode -- UseSharedMailbox (Boolean)

| Mode | `UseSharedMailbox` | Description | Required Properties | AI-XAML Suitable |
|------|--------------------|-------------|--------------------|--------------------|
| Own Mailbox | `False` (default) | Operate on the connected user's own mailbox. | None | Yes |
| Shared Mailbox | `True` | Operate on a shared mailbox the user has access to. | `SharedMailbox` | Yes |

## Properties

| Property | Type | Description |
|----------|------|-------------|
| `UseSharedMailbox` | `Boolean` | Set to `true` to access a shared mailbox. Default: `false`. |
| `SharedMailbox` | `InArgument<String>` | The email address of the shared mailbox. Required when `UseSharedMailbox` is `true`. |
| `Backup` | `BackupSlot<MailboxSelectionMode>` | Stores the previous mode for undo. |

## Resolution Priority

At runtime, the mailbox is resolved in this order:
1. If the Integration Service connection specifies a shared mailbox, that takes priority.
2. If `UseSharedMailbox` is `true`, the `SharedMailbox` email address is used.
3. Otherwise, the authenticated user's own mailbox is used.

## XAML Examples

### Own Mailbox (default)
```xml
<mailmodels:MailboxArgument UseSharedMailbox="False" />
```

### Shared Mailbox
```xml
<mailmodels:MailboxArgument UseSharedMailbox="True">
    <mailmodels:MailboxArgument.SharedMailbox>
        <InArgument x:TypeArguments="x:String">["shared-inbox@contoso.com"]</InArgument>
    </mailmodels:MailboxArgument.SharedMailbox>
</mailmodels:MailboxArgument>
```

## Variant: TriggerMailboxArgument

A simplified version used by mail triggers. The `SharedMailbox` property is a plain `String` instead of `InArgument<String>`.

| Property | Type | Description |
|----------|------|-------------|
| `UseSharedMailbox` | `Boolean` | Set to `true` to access a shared mailbox. |
| `SharedMailbox` | `String` | The email address of the shared mailbox. |

## Cross-References

- Used by Outlook mail activities (SendMailConnections, GetEmailListConnections, ForEachEmailConnections, etc.)
- Works alongside [MailFolderArgument](MailFolderArgument.md) for folder selection
- Emails are of type [Office365Message](../types/Office365Message.md)
