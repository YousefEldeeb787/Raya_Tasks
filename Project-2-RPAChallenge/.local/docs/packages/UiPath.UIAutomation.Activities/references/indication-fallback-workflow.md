# Indication Fallback Workflow

Manual fallback for Object Repository target registration when elements appear only after user interaction (e.g., a compose form that opens after clicking a button), so `uia-configure-target`'s automated capture cannot see them. Both commands require the user to physically click on the target.

## Before indicating

Indication operations are **screen-blocking and user-driven**. The CLI takes over the screen with an inspection overlay and blocks until the user clicks something or cancels. Before issuing either command:

1. **Describe the target to the user.** Tell them, in concrete terms, what to click — the application, the screen, the specific element / control / window. Do not assume the target is already in front of them.
2. **Wait for the user to acknowledge readiness** before invoking. Use whatever confirmation tool the agent harness provides (e.g., `AskUserQuestion`). Do not invoke the indicate command speculatively — issuing it while the user is still navigating means the overlay captures whatever happens to be on screen at that instant.
3. **Do not wrap the call in a short shell timeout.** Indication is user-driven and routinely takes minutes. A short timeout kills the call before the user can react.

## Workflow

Indicate the screen first, then indicate elements within it using the screen's `--parent-id`.

```bash
# 1. Indicate a screen (creates App automatically if none exists).
#    Run only after the user has confirmed they are ready.
uip rpa indicate-application \
  --name "<ScreenName>" \
  --description "<ScreenDescription>" \
  --project-dir "<PROJECT_DIR>" \
  --output json

# 2. Indicate elements on that screen (use --parent-id from step 1 result's Data.reference).
#    Run only after the user has confirmed they are ready.
uip rpa indicate-element \
  --name "<ElementName>" \
  --activity-class-name "<TypeInto|Click|GetText|...>" \
  --parent-id "<screen-reference>" \
  --project-dir "<PROJECT_DIR>" \
  --output json
```

Both commands return:

```json
{ "Data": { "reference": "..." } }
```

Use the returned reference ID for Object Repository lookups and target attachment.

## After indication

Studio regenerates the Object Repository files. Subsequent steps depend on workflow type:

- **Coded workflows** — re-read `ObjectRepository.cs` to get updated descriptor paths (`Descriptors.<App>.<Screen>.<Element>`).
- **XAML workflows** — attach each reference to its activity per `uia-target-attachment-guide.md` (sibling file in this `references/` folder).

## Pitfalls

- **Do not use `--parent-name` with the App display name** (e.g., `"Acme"`) — it matches AppVersion names (e.g., `"1.0.0"`), not App names. Pass `--parent-id` with the AppVersion reference instead.
- **Do not use the App `_reference` from `ObjectRepository.cs` as `--parent-id`** — that is the App reference, not the AppVersion reference. Read `.objects/` metadata for the AppVersion reference.

## Full parameter reference

For every flag, troubleshooting entries, and additional examples: `cli-reference.md` § Indicate (sibling file in this `references/` folder).
