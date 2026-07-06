---
name: uia-improve-selector
description: "Fix, improve, or recover a UiPath selector using runtime data. **Use when** (1) a selector stopped working and user has runtime data, (2) user asks to fix/improve/recover a selector, (3) user mentions 'element not found' with a snapshot or runtime data folder, (4) user wants to make an existing selector more robust, (5) a selector needs to be fixed after a failure. **Example phrases** 'fix this selector', 'selector stopped working', 'improve the selector', 'element not found, here\\'s the runtime data', 'make this selector more robust', 'recover this selector', 'element not found'"
argument-hint: "<definition-file> --folder <path> [--mode <recover|improve>] [--quiet] [--project-dir <path>]"
allowed-tools: Bash, Read, Write, AskUserQuestion
---

Fix or improve a UiPath selector using the UiAutomation CLI and runtime data.

The skill operates on a definition file (XAML) in `$FOLDER`. Callers pass a `$DEF_FILE` directly. Supports both full element-mode and window-only modes.

**IMPORTANT: Use forward slashes in ALL paths.** Backslash paths break the Read tool.

## CLI

Define a shell **function** named `cli` that wraps the CLI, then call it as `cli <subcommand> ...` everywhere below:

```bash
cli() { uip rpa uia "$@"; }
```

If `$PROJECT_DIR` is set, define it with the project dir baked in instead:

```bash
cli() { uip rpa uia --project-dir "$PROJECT_DIR" "$@"; }
```

Either way, every `cli ...` call below automatically uses the right base command and `--project-dir`. Using a function (not a `CLI="..."` string variable) keeps argument boundaries intact when any path -- `--project-dir`, `--folder-path`, or `--definition-file-path` -- contains spaces.

> **WARNING -- never put the command in a string variable. Re-expanding a command from a string breaks on any path containing spaces. Use the `cli` function, or write `uip rpa uia ...` out in full with every path double-quoted at the call site. Re-declare the `cli` function at the top of each Bash invocation that calls it (shell state does not persist between tool calls).
>
> **Misleading error decode.** If the CLI reports `'uia' requires the 'UiPath.UIAutomation.CLI' package`, this is almost always a **bad or truncated `--project-dir`** (commonly a project path with spaces that got word-split), **not** a real missing-package problem. Verify `--project-dir` arrived as a single quoted argument before touching package versions.

## Input Parsing

Extract from `$ARGUMENTS`:

- First positional argument or `--definition` -> `$DEF_FILE`. Path to the definition file (XAML) containing the `TargetApp` or the `TargetAnchorable`.
- `--folder <path>` -> `$WORK_FOLDER`. Folder for snapshot artifacts (tree JSONs, screenshot, instruction files).
- `--mode recover` or `--mode improve` -> `$MODE`. If not specified, infer from phrasing:
  - **recover** (default): "fix", "broken", "stopped working", "element not found", "recover", "failed"
  - **improve**: "improve", "robust", "optimize", "strengthen", "harden", "resilient"
  - If unclear, default to `recover`.
- `--quiet` -> `$QUIET=true` (default: `false`). Suppress all output -- just write files. Used when this skill is called as a sub-step by another skill.
- `--project-dir <path>` -> `$PROJECT_DIR` (optional). UiPath project directory. Included in all CLI commands via the `cli` function.

If `$WORK_FOLDER` is not provided and `$DEF_FILE` is provided, use the directory containing `$DEF_FILE`. If `$DEF_FILE` is not provided, ask the user for one.

### Refresh snapshot artifacts

Run `snapshot capture` to fill any gaps and refresh the highlighted screenshot when the app is live:

```bash
cli snapshot capture --folder-path "$WORK_FOLDER" --definition-file-path "$DEF_FILE"
```

`snapshot capture` skips tree extractions for files that already exist, so pre-staged offline folders are preserved. A live app adds a fresh `ApplicationScreenshot.jpg` with the target highlighted; an unreachable app just means no updated screenshot.

## IMPROVE-1: Get Instructions

Run the CLI to generate instruction files. The CLI writes three separate files to `$WORK_FOLDER`:
- `selector-system-prompt.md` -- the system prompt with rules and constraints
- `selector-user-message.md` -- the user message with the task, selectors, and DOM data
- `selector-schema-config.md` -- the JSON schema for the expected output

```bash
cli selector-intelligence get-instructions --folder-path "$WORK_FOLDER" --definition-file-path "$DEF_FILE" --mode "$MODE"
```

## IMPROVE-2: Read Instructions and Generate Selectors

**Use ONLY the Read tool and Write tool for this step. Do NOT use Bash, Python, or any other tool to parse, process, or extract content from the instruction files.**

1. Read the three instruction files using the Read tool (use `offset` and `limit` for large files):
   - `$WORK_FOLDER/selector-system-prompt.md` -- the rules and constraints to follow
   - `$WORK_FOLDER/selector-user-message.md` -- the task to execute (can be large due to DOM JSON)
   - `$WORK_FOLDER/selector-schema-config.md` -- the output schema
2. Read `$WORK_FOLDER/ApplicationScreenshot.jpg` for visual context (skip if it doesn't exist).
3. Execute the task from `selector-user-message.md` following the rules from `selector-system-prompt.md`. Write the JSON result (conforming to the schema in `selector-schema-config.md`) to `$WORK_FOLDER/improve-selector-output.json` using the Write tool.

## IMPROVE-3: Validate and Retry Loop

This step runs a validate->fix loop. **Max 3 iterations total.**

Set `$ATTEMPT = 1`.

### Validate

```bash
cli selector-intelligence validate --folder-path "$WORK_FOLDER" --definition-file-path "$DEF_FILE" --improve-selector-response-file-path "$WORK_FOLDER/improve-selector-output.json" --mode "$MODE" > "$WORK_FOLDER/validation-result.txt" 2>&1
```

Read `$WORK_FOLDER/validation-result.txt`.

### At least one valid -> done

Pick the selector with the highest FinalScore. Read the top-level `reasoning` field from `$WORK_FOLDER/improve-selector-output.json` to extract the root cause and strategy.

Apply the winning candidate to `$DEF_FILE` with the appropriate `update-definition` CLI command.

From the winning candidate, read:
- `$WINDOW_SELECTOR` = the candidate's `WindowSelector`.
- `$EDITABLE_PARTIAL_SELECTOR` = the candidate's `EditablePartialSelector` (may be absent — leave it unset in that case).

**Element definition** (`TargetAnchorable`) — set the scope selector to `$WINDOW_SELECTOR`, and the full selector to `$EDITABLE_PARTIAL_SELECTOR`. **Only pass `--full-selector` when the candidate actually has an `EditablePartialSelector`** — passing `--full-selector ""` would overwrite the existing full selector with an empty value.

When the candidate has an `EditablePartialSelector`:

```bash
cli target-anchorable update-definition \
  --definition-file-path "$DEF_FILE" \
  --scope-selector "$WINDOW_SELECTOR" \
  --full-selector "$EDITABLE_PARTIAL_SELECTOR"
```

When the candidate has no `EditablePartialSelector`, omit the option entirely:

```bash
cli target-anchorable update-definition \
  --definition-file-path "$DEF_FILE" \
  --scope-selector "$WINDOW_SELECTOR"
```

**Window definition** (`TargetApp`) — set the selector to `$WINDOW_SELECTOR`:

```bash
cli target-app update-definition \
  --definition-file-path "$DEF_FILE" \
  --selector "$WINDOW_SELECTOR"
```

Jump to **Output**.

### None valid -> fix and retry

If `$ATTEMPT >= 3`: save the last validation result and jump to **Output** with the errors.

Otherwise:

1. Read `$WORK_FOLDER/validation-result.txt` carefully -- focus on the `ToolingFeedback` for each candidate to understand what went wrong (schema violations, invalid attributes, missing required fields, etc.).
2. Re-read `$WORK_FOLDER/improve-selector-output.json` to see what you generated.
3. Fix the generation based on the feedback: adjust the selectors to address the specific issues flagged. Write the corrected result to `$WORK_FOLDER/improve-selector-output.json` (overwrite).
4. Increment `$ATTEMPT` and go back to **Validate**.

## Output

**If `$QUIET` is `true`: The calling skill will continue with its next step.**

**If no valid selectors after 3 attempts:** present the last validation errors and stop improving attempts.

**Otherwise:** read `$DEF_FILE` to get the selectors. Present using this template (fixed selector LAST so it's visible in terminal):

````
---
### Selector <Fixed|Improved>  (Score: <FinalScore>/1.0)

> **Root cause:** <one sentence -- why the original selector broke>  <- recover mode only

> **Strategy:** <one sentence -- what makes the selector more robust>

> <If there's a score penalty, add one line explaining it's a structural UI property, not fixable by selector changes.>

**Original:**
```
<Selector or ScopeSelectorArgument from the definition file XAML>
<each tag on its own line from PartialSelector>  <- omit if empty
```

**Window:**
```xml
<WindowSelector from validation result>
```

**Target:**  <- omit entire block if no EditablePartialSelector
```xml
<each tag on its own line from EditablePartialSelector>
```
---
````

- If the window selector didn't change from the original, omit it from both **Original:** and **Window:** sections.
- Keep it tight -- no full analysis dump. The detailed candidate analysis is already saved in the output files if needed.
- Do NOT show all 3 selectors. Do NOT retry for score penalties -- they're often structural properties of the UI that can't be fixed by adding tags.
