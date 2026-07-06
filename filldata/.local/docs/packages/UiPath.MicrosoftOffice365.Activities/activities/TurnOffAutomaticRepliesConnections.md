# Turn Off Automatic Replies

`UiPath.MicrosoftOffice365.Activities.Mail.TurnOffAutomaticRepliesConnections`

Turns off automatic replies (Out of Office) for the user's mailbox.

| Property | Value |
|---|---|
| **Package** | `UiPath.MicrosoftOffice365.Activities` |
| **Category** | Mail |
| **Connector** | `uipath-microsoft-outlook365` |

## Input Properties

This activity has no input properties.

## Output Model

This activity does not return an output object.

## XAML Example

```xml
<mail:TurnOffAutomaticRepliesConnections
    DisplayName="Turn Off Automatic Replies" />
```

## Notes

- Requires the `MailboxSettings.ReadWrite` scope.
