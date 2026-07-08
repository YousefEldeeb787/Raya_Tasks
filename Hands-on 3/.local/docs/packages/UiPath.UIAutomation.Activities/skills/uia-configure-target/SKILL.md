---
name: uia-configure-target
description: "Primary entry point for configuring a UiPath target -- ensures the screen and element exist in the Object Repository, checking for existing entries before creating new ones. Returns the OR reference ID. Supports batch element configuration via pipe-separated list (e.g., --elements \"Five button | Plus button | Equals button\") to avoid redundant window captures and screen lookups. Use when asked to 'configure target', 'configure application', 'set up target', 'set up application', 'create target in OR', 'find or create target', 'get OR reference for an element', 'select application window', 'create window selector', 'create selector', 'get selector for', 'find selector', 'add target to object repository', or when an orchestrator agent needs an OR element reference for a UI element. Trigger this whenever building automation workflows that need reliable OR references."
argument-hint: "--window <description> [--elements <descriptions>] [--semantic] [--no-improve] [--activity <type>] [--project-dir <path>]"
allowed-tools: Bash, Read, Write, Agent, AskUserQuestion
---

Ensure a UI target (screen + elements) exists in the Object Repository. Checks for existing OR entries first -- creates new ones only when needed. Returns the OR reference ID(s).

`$ARGUMENTS` format: `--window <description> [--elements <descriptions>] [--semantic] [--no-improve] [--activity <type>] [--project-dir <path>]`

**IMPORTANT: Use forward slashes in ALL paths.**

**IMPORTANT: The CLI is a native OS executable — every path passed to it (`--folder-path`, `--definition-file-path`, `--project-dir`, `--refs` JSON, etc.) must be an absolute path in the host OS's own format.** On Windows that means a drive-letter path like `C:/Work/.../folder`. Do **not** pass a shell-emulation path such as a git-bash/MSYS/Cygwin `pwd` result (`/c/Work/...`), a WSL `/mnt/c/...` path, or any relative path — the native CLI cannot resolve these and will silently write artifacts to the wrong place or fail with confusing "path not found" errors. When you need to derive an absolute path inside git-bash on Windows, use `pwd -W` (or `cygpath -m`), which yields the `C:/...` forward-slash form, **never** a bare `pwd`.

**IMPORTANT: Follow the steps mechanically. Do NOT add commentary or analysis between steps.**

**IMPORTANT: For full details on Object Repository concepts (Application, Screen, Element) and the complete CLI command reference, see [`object-repository.md`](../../references/object-repository.md).**

## CLI

Define a shell **function** named `cli` that wraps the CLI, then call it as `cli <subcommand> ...` everywhere below:

```bash
cli() { uip rpa uia "$@"; }
```

If `$PROJECT_DIR` is set, define it with the project dir baked in instead:

```bash
cli() { uip rpa uia --project-dir "$PROJECT_DIR" "$@"; }
```

Either way, every `cli ...` call below automatically uses the right base command and `--project-dir`. Using a function (not a `CLI="..."` string variable) keeps argument boundaries intact when any path -- `--project-dir`, `--folder-path`, `--definition-file-path`, or a `--refs` JSON value -- contains spaces.

> **WARNING -- never put the command in a string variable. Re-expanding a command from a string breaks on any path containing spaces. Use the `cli` function, or write `uip rpa uia ...` out in full with every path double-quoted at the call site. Re-declare the `cli` function at the top of each Bash invocation that calls it (shell state does not persist between tool calls).
>
> **JSON arguments** (`--refs`, `--definition-file-paths`): keep each path inside the double-quoted JSON literal exactly as shown at each call site, and never reconstruct the JSON by re-expanding a bare variable -- that re-introduces the same word-splitting bug.
>
> **Misleading error decode.** If the CLI reports `'uia' requires the 'UiPath.UIAutomation.CLI' package`, this is almost always a **bad or truncated `--project-dir`** (commonly a project path with spaces that got word-split), **not** a real missing-package problem. Verify `--project-dir` arrived as a single quoted argument before touching package versions.

## Input Parsing

Extract from `$ARGUMENTS`:

- `--window <description>` -> `$WINDOW`. Window/tab description to target.
- `--elements <descriptions>` -> pipe-separated list of target element descriptions (optional). Also accepts `--element`. Use `|` to separate multiple elements (e.g., `"Five button | Plus button | Equals button"`). If omitted, run in **screen-only mode**.
- `--semantic` -> `$CONFIGURE_SEMANTIC=true` (default: `false`). Enable Semantic (NLP) secondary targeting. Ignored in screen-only mode.
- `--no-improve` -> `$NO_IMPROVE=true` (default: `false`). Skip the uia-improve-selector subagent (6.3).
- `--activity <type>` -> `$ACTIVITY_TYPE` (default: `Click`). Valid values: `Click`, `GetText`, `SetText`, `TypeInto`, `Check`, `Hover`, `Highlight`, `SelectItem`, `GetAttribute`, `TakeScreenshot`, `KeyboardShortcut`, `MouseScroll`, `DragAndDrop`, `InjectJsScript`, `CheckState`, `FindElements`, `SetFocus`, `CheckElement`, `ElementScope`, `WindowOperations`.
- `--project-dir <path>` -> `$PROJECT_DIR` (optional). UiPath project directory. Passed through to all CLI commands and subagent prompts.

If `$WINDOW` is not provided, ask the user which application/window to target.

**Parse elements:** Split the `--elements` value on `|` and trim whitespace from each entry to produce `$ELEMENT_LIST` (array). Derive `$ELEMENT_NAMES` by converting each entry to Title Case (e.g., "add to cart button" -> `Add To Cart Button`).

Derive `$SCREEN_NAME` from `$WINDOW` by converting to Title Case (e.g., "google chrome" -> `Google Chrome`).

## Definition Files

A **definition** describes a single window or element and is split into two on-disk parts: a XAML file that holds the runtime activity object (a `TargetApp` or `TargetAnchorable` -- see [Target.md](../../activities/common/Target.md)) and a sibling `.xaml.metadata` JSON file that holds the design-time metadata (names, ids, activity type, etc.). The CLI reads/writes both as a pair.

**The `.xaml.metadata` file MUST always exist alongside the `.xaml` file and share the same base name** (e.g., `window.xaml` and `window.xaml.metadata`). One never appears without the other.

**Window definition** -- describes the application window/screen. Used for OR screen registration.

- `window.xaml` -- a serialized [`TargetApp`](../../activities/common/Target.md#targetapp). Created by `target-app resolve-default` in TARGET-2.
- `window.xaml.metadata` -- JSON metadata file: `WindowNodeId`, `Name`, `Description`.

**Element definition** -- describes a single UI element inside a window. Used for OR element registration.

- `target-n.xaml` -- a serialized [`TargetAnchorable`](../../activities/common/Target.md#targetanchorable). Created in TARGET-6 by `target-anchorable resolve-default`, which seeds the scope selector from `window.xaml`.
- `target-n.xaml.metadata` -- JSON metadata file: `WindowNodeId`, `ElementNodeId`, `ActivityType`, `Name`, `Description`.

**IMPORTANT:** Definition files are ALWAYS generated by the CLI -- `cli target-app resolve-default ...` for a window definition or `cli target-anchorable resolve-default ...` for an element definition. **Never create or hand-edit `.xaml` or `.xaml.metadata` files yourself.** Once created, the only supported way to mutate a definition file is `cli target-app update-definition ...` or `cli target-anchorable update-definition ...` with a selector generated by TARGET-6. Both rewrite the XAML and the metadata atomically. You MAY read `.xaml` and `.xaml.metadata` files to inspect selectors and metadata.

## Error Handling

After every CLI command, check the exit code. If non-zero, show the CLI's stderr/stdout to the user and stop. Common failures:
- **snapshot capture**: application not running, window minimized, or not visible on screen
- **target-app resolve-default / target-anchorable resolve-default**: invalid ref or element not found in tree
- **ref resolution** (`Target eN not found in snapshot` / `Target eN is stale`): no snapshot is currently loaded for that ref kind (capture/inspect first), or the ref targets an element that is gone / belongs to an older snapshot that has since been replaced.

## TARGET-1: Prepare Working Folder

Always start from a clean folder -- never reuse a previous run's folder. Stale artifacts from a prior run may reference a different window or app state.

```bash
rm -rf .local/.uia/.configure-target
mkdir -p .local/.uia/.configure-target
```

Set `$WORK_FOLDER` to the **absolute path** of `.local/.uia/.configure-target` (the CLI requires absolute paths). Resolve it in the host OS's native format — on Windows, derive it from `pwd -W` (or `cygpath -m "$(pwd)"`) so you get a `C:/...` drive-letter path, **not** a bare git-bash `pwd` (`/c/...`), which the native CLI cannot resolve (see the path note at the top of this file). Verify the resulting `$WORK_FOLDER` starts with a drive letter before passing it on.

## TARGET-2: Create Window Selector

Capture the top-level tree. No definition file exists yet, so omit `--definition-file-path` -- the CLI captures the full top-level snapshot without scoping to a target:

```bash
cli snapshot capture --folder-path "$WORK_FOLDER"
```

This produces a rendered `window-tree.md`.

View the window tree:

```bash
cat "$WORK_FOLDER/window-tree.md"
```

Read the output. Match `$WINDOW` against window titles and app names (partial, case-insensitive). Browser tabs are labeled `BrowserTab` with `b` prefix refs (e.g., `b3`) -- prefer those over native browser windows for web apps. Regular windows use `w` prefix refs (e.g., `w3`).

Save the matching ref as `$WREF`. If no match, present the list and ask the user.

Get the window selector (the CLI creates the default `TargetApp` with the default selector on the `Selector` and writes the XAML to `window.xaml` + `WindowNodeId` in the `window.xaml.metadata`). Pass `--refs` as a JSON literal; the forward-slash rule above keeps the JSON safe (Windows backslashes would be interpreted as JSON escapes):

```bash
cli target-app resolve-default --folder-path "$WORK_FOLDER" \
  --refs "[{\"ref\":\"$WREF\",\"definitionFilePath\":\"$WORK_FOLDER/window.xaml\"}]" \
  --name "$SCREEN_NAME" --from-snapshot
```

**Stabilize title if needed:** Read `$WORK_FOLDER/window.xaml` and inspect the `Selector`'s `title` attribute (if present). The `title` often reflects the current page content (e.g., article headline, search query, email subject) rather than just the application identity. If the title contains volatile page-specific content beyond the app name, simplify it — keep only the stable app-identifying portion with wildcards (e.g., `title='*10 Unread - dan@ - Outlook*'` → `title='*Outlook*'`). The kept portion must be a substring of the original title value (ignoring wildcards) so it still matches the current window. If the title already looks stable (e.g., a desktop app like `title='Calculator'`), leave it as-is. If a change is needed, use the CLI to write it back:

This hand-edit is allowed **only for the window selector**, not for element selectors — those come only from TARGET-6

```bash
cli target-app update-definition \
  --definition-file-path "$WORK_FOLDER/window.xaml" \
  --selector "$STABILIZED_WINDOW_SELECTOR"
```

**If `$ELEMENT_LIST` is not empty**, capture the app-level tree now (window selector is set):

```bash
cli snapshot capture --folder-path "$WORK_FOLDER" --definition-file-path "$WORK_FOLDER/window.xaml"
```

This produces `ApplicationLevelNodeTreeInfo.json`, `ApplicationLevelApplicationMetadata.json`, and `ApplicationScreenshot.jpg`.

## TARGET-3: Search for Screen in Object Repository

```bash
cli object-repository get-screens --definition-file-path "$WORK_FOLDER/window.xaml"
```

Initialize `$SCREEN_REF_ID` to empty.

**If the table has rows:** compare each row against `$WINDOW` to find the best match:

- **Name match** (case-insensitive): strong signal.
- **Selector match**: if the stored window selector targets the same application and window title, strong signal.

**Confident match found:** save the screen's `ReferenceId` as `$SCREEN_REF_ID`.

**Multiple plausible matches:** list the candidates and ask the user to pick.

**If the table is empty or the command fails** -- leave `$SCREEN_REF_ID` empty.

**Screen-only mode** (no `--elements`): skip to TARGET-6.

## TARGET-4: Search for Existing Elements in Object Repository

**Skip if `$SCREEN_REF_ID` is empty** (no screen found -- elements can't exist). Mark all elements as needing creation and proceed to TARGET-5.

```bash
cli object-repository get-elements --screen-reference-id "$SCREEN_REF_ID"
```

**If elements exist:** compare each row against EVERY element in `$ELEMENT_LIST` to find matches:

- **Name match** (case-insensitive, allowing minor wording differences): strong signal.
- **Semantic selector match**: if the stored semantic description refers to the same UI element, strong signal.
- **Selector match**: if the stored selector targets the same control type with similar identifying attributes, supporting signal.
- If a screenshot file path is present and the match is uncertain, read the screenshot for visual confirmation.

For each element: **confident match** -> record `{$ELEMENT_NAME, $ELEMENT_REF_ID, found}` (skips TARGET-5 through TARGET-9). **No match** -> mark as needing creation.

Collect elements needing creation into `$ELEMENTS_TO_CREATE` (list of `{$INDEX, $ELEMENT, $ELEMENT_NAME}`).

If `$ELEMENTS_TO_CREATE` is empty, skip to **Output**.

## TARGET-5: Identify Element Reference

`snapshot capture` (TARGET-2) wrote `$WORK_FOLDER/tree.md`. Each visible node is rendered on one line:

```
  - Button [ref=e42] "Add to cart"
  - InputBox [ref=e15] [sap] "Username"
```

Use `Grep` (with regex if needed) as the primary discovery tool, then `Read` with `offset`/`limit` around grep hits to inspect surrounding context. The tree may be tens of thousands of lines — never read it unbounded. Examples:

```
Grep pattern="Add to cart" path="$WORK_FOLDER/tree.md" output_mode="content"
Grep pattern="(?i)username.*\[ref=" path="$WORK_FOLDER/tree.md" output_mode="content"
```

The `[sap]` tag indicates the element belongs to a SAP web framework (Fiori/UI5, Web GUI, or Ariba). SAP framework elements carry richer, more stable attributes that produce more reliable selectors.

**SAP element selection rule:** Prefer the `[sap]`-tagged node over its plain child/parent when **they share the same Name** and sit **within 1–2 tree levels** of each other — they are the same logical element. This holds even when the `[sap]` node has a generic role (`Container`, `Group`) and the plain inner is the native control (`InputBox`, `Button`, `INPUT`). The `[sap]` node's selector probes the richer SAP framework attributes, which are more stable than the inner native control's attributes.

Match each element description to a tree result and save its ref as `$EREF_N`.

### Retry capture with UIA framework

If any element could not be confidently matched in the tree (no grep hits, or hits exist but none correspond to the described element), the default capture framework may have produced a tree that lacks those elements. Before retrying, check whether a different framework would help:

1. Read `$WORK_FOLDER/ApplicationLevelApplicationMetadata.json` and check the `"subsystem"` field.
2. **Only retry if `subsystem` is `"aa"`** (Active Accessibility) — UIA queries a different accessibility provider and may surface elements that AA missed. For any other subsystem (`"uia"`, `"webctrl"`, `"html"`, `"java"`, etc.), the framework-specific tree is richer than what UIA would produce. Skip the retry and proceed to screenshot disambiguation.

To retry:

```bash
rm -f "$WORK_FOLDER/ApplicationLevelNodeTreeInfo.json" "$WORK_FOLDER/ApplicationLevelApplicationMetadata.json" "$WORK_FOLDER/ApplicationScreenshot.jpg" "$WORK_FOLDER/tree.md"
cli snapshot capture --folder-path "$WORK_FOLDER" --definition-file-path "$WORK_FOLDER/window.xaml" --framework uia
```

The recapture overwrites `tree.md`. Treat any previously recorded `eN` refs as unreliable against the new snapshot (they may resolve to a different element or become stale). Re-grep against the new tree and continue matching.

**If any element has multiple candidates or no clear match from the tree alone**, read the screenshot once to disambiguate:

```
Read "$WORK_FOLDER/ApplicationScreenshot.jpg"
```

Cross-reference the screenshot (visual) with the tree results (structural) to resolve the ambiguity. If still ambiguous after checking the screenshot, list candidates and ask the user.

## TARGET-6: Get Element Selectors

**IMPORTANT**: Every element selector must be produced by `resolve-default` or the uia-improve-selector subagent. Those steps are the only source of a selector; work through them until it is `RELIABLE`. Never write an element selector with `target-anchorable update-definition --full-selector` (e.g. if you have the attributes from `interact get-all`). A selector that looks right to you but wasn't produced by TARGET-6 is **unverified**, however correct its attributes seem.

**To parametrize a selector on an argument** (e.g. any runtime text that appears in a common text attribute — `aaname`, `name`, `text`, etc.), do not write the `{{variable}}` yourself — run the uia-improve-selector subagent (6.3) and name the attribute to parametrize in its `$ATTR_INFO`. The subagent is the only approved way to produce a parametrized selector.

### 6.1: Get the default selector

**Do NOT use `--from-snapshot` here.** Element selectors must probe the live element. Each call seeds its `ScopeSelectorArgument` from the `TargetApp`'s `Selector` from the `window.xaml` and resolves the default `TargetAnchorable` into the element's definition file.

Pass the refs as an inline JSON array (one object per element; each carries its own `activityType`):

```bash
cli target-anchorable resolve-default --folder-path "$WORK_FOLDER" \
  --window-definition-file-path "$WORK_FOLDER/window.xaml" \
  --refs "[{\"ref\":\"$EREF_1\",\"definitionFilePath\":\"$WORK_FOLDER/target-1.xaml\",\"activityType\":\"$ACTIVITY_TYPE\"},{\"ref\":\"$EREF_2\",\"definitionFilePath\":\"$WORK_FOLDER/target-2.xaml\",\"activityType\":\"$ACTIVITY_TYPE\"}]"
```

### What improvement can act on

Selector improvement operates **only** on the `FullSelectorArgument` — the selector used by the `Selector` (strict) search step — of a main target (`TargetAnchorable`) or an anchor (`Target`). It does **NOT** improve any other targeting method (`FuzzySelector`, `SemanticSelector`, `CV`, `TextNative`, `Image`).

### Assess selector reliability

**Screen-only mode:** Read `$WORK_FOLDER/window.xaml` and assess the `Selector` from the `TargetApp`.

**Element mode:** For each element in `$ELEMENTS_TO_CREATE`, read `$WORK_FOLDER/target-${INDEX}.xaml` and assess the `FullSelectorArgument` from the `TargetAnchorable`. Also assess the `ScopeSelectorArgument` once (from the first such element).

**Assessment criteria -- a selector is RELIABLE if ALL of the following hold:**

1. **Uses reliable attributes for its tag type.** Each tag has at least one developer-assigned or semantic identifier (e.g., `automationid`, `name`, `role`, `aria-label`, `id`, `app`, `cls`). Fragile if all identifying attributes are last-resort or unreliable for their tag type.

2. **Not positionally dependent.** The selector does NOT rely on `idx`, `tableRow` or `tableCol` or other attributes that are purely positional.

3. **Attribute values are stable.** Watch for auto-generated IDs (purely numeric like `id='89763184740'`), CSS-in-JS hashes (`class='css-1wq41pf'`), component-path IDs with 3+ dot-separated structural segments, or framework hashes in tag names.

4. **Activity-appropriate attributes.** For `GetText`/`SetText`/`TypeInto`: must NOT use content-reflecting attributes (`text`, `aaname`, `visibleinnertext`, `innertext`) as primary identifiers. For `Check`/`Uncheck`: must NOT rely on state attributes (`checked`, `aastate`). For `SelectItem`: must NOT rely on `selecteditem` or `value`.

5. **Good structure.** A typical selector has ~2 tags. Selectors with 4+ tags are over-specified and fragile. Each tag should have 2-3 meaningful attributes.

6. No `css-selector` attribute.

Mark each selector as `RELIABLE` or not.

**If all selectors are RELIABLE:** skip improvement entirely and proceed to TARGET-7.

### 6.2: Retry with snapshot before improving

**Skip for the window selector** (window selectors always use `--from-snapshot` -- nothing different to try).

Do NOT skip directly to the improvement subagent. Every element not RELIABLE MUST first go through one --from-snapshot retry and re-assessment. The subagent is reserved for elements that remain not RELIABLE after this retry. This retry is one cheap CLI call; its re-assessment is the decision gate for whether the more expensive subagent is warranted. Do not predict its outcome — run it.
 
Collect all element refs not marked `RELIABLE`. Read each element's `target-${INDEX}.xaml` to get its `ElementNodeId`.

**Batch the snapshot retry in a single CLI call** with an inline JSON array (omit `activityType` here — snapshot mode doesn't need it). The CLI writes each new selector directly into the definition file:

```bash
cli target-anchorable resolve-default --folder-path "$WORK_FOLDER" \
  --window-definition-file-path "$WORK_FOLDER/window.xaml" \
  --refs "[{\"ref\":\"$EREF_1\",\"definitionFilePath\":\"$WORK_FOLDER/target-1.xaml\"},{\"ref\":\"$EREF_2\",\"definitionFilePath\":\"$WORK_FOLDER/target-2.xaml\"}]" \
  --from-snapshot
```

Re-read each updated `target-${INDEX}.xaml` and re-assess the new `FullSelectorArgument` using the same criteria above.

If the new selector is `RELIABLE`, mark it as such and skip improvement for it.

**Restore live-probe selectors for all non-RELIABLE elements.** The `--from-snapshot` call overwrote the definition files. For every element that was NOT promoted to `RELIABLE` by the snapshot selector, call `target-anchorable resolve-default` again **without** `--from-snapshot` to restore the original live-probe selector:

```bash
cli target-anchorable resolve-default --folder-path "$WORK_FOLDER" \
  --window-definition-file-path "$WORK_FOLDER/window.xaml" \
  --refs "[{\"ref\":\"$EREF_X\",\"definitionFilePath\":\"$WORK_FOLDER/target-x.xaml\",\"activityType\":\"$ACTIVITY_TYPE\"},{\"ref\":\"$EREF_Y\",\"definitionFilePath\":\"$WORK_FOLDER/target-y.xaml\",\"activityType\":\"$ACTIVITY_TYPE\"}]"
```

**If all selectors are now RELIABLE:** skip improvement entirely and proceed to TARGET-7.

### 6.3: Run improvement on fragile selectors only

**Skip if `$NO_IMPROVE` is true.** Proceed to TARGET-7.

**Mandatory — not a judgment call.** Every element still not RELIABLE after 6.2 MUST go through improvement here.

**Not an alternative to TARGET-8**; it runs first. A selector improved to RELIABLE drops out of anchoring entirely (8.2 Rule 4), the most stable outcome, so you can't know which elements still need anchoring until 6.3 has run. Don't predict which will improve from its attributes — run it.

Collect only the elements not marked `RELIABLE` into `$ELEMENTS_TO_IMPROVE`. If the window selector is not `RELIABLE`, include it too.

**Gate - blocking — strict `Selector` required.** Before proceeding to subagent based improving, print the `SearchStep` of each element in `$ELEMENTS_TO_IMPROVE`, using its `TargetAnchorable`. You *may not* spawn a subagent for an element if its `SearchSteps` does not contain `Selector`. Do not infer or assume the value, read it from the previous output. **If `SearchSteps` of an element does not contain `Selector`, do NOT run the subagent for that element** — remove it from `$ELEMENTS_TO_IMPROVE`. (The window `TargetApp` selector is always strict, so this never excludes the window.). Elements from `$ELEMENTS_TO_IMPROVE`, that pass the guard **MUST** go through step 6.3 - again, do not predict the outcome, run it. Elements excluded here (SearchSteps = FuzzySelector) are not improved by the subagent at all — they are stabilized by anchoring in TARGET-8.

For each element in `$ELEMENTS_TO_IMPROVE`, seed the subagent with the selector (from 6.1 or 6.2) that points to it. The subagent only hardens the element its seed points to — it does not look for a better element — so prefer a unique but volatile selector over a too-generic one. Restore it by re-running that step's `resolve-default`.

**Screen-only mode (window needs improvement):** Spawn a single subagent. Use `$DEF_FILE = $WORK_FOLDER/window.xaml` and `$AGENT_FOLDER = $WORK_FOLDER` (no isolation needed -- only one agent runs).

**Element mode:** Spawn one `Agent` per element in `$ELEMENTS_TO_IMPROVE`, **all in a single message** so they run in parallel. Each improves window + element together. If the window selector is `RELIABLE` but some elements need improvement, the subagent still receives the window selector in the definition -- it will only change the element selector.

**Isolate per-agent artifacts.** The uia-improve-selector CLI writes fixed-name artifacts into its `--folder`. Parallel agents pointing at the same folder would overwrite each other. Give each agent its own subfolder, seeded with the already-captured snapshot so it skips re-capture:

```bash
# Element mode only -- run once before spawning agents
for INDEX in <indices of $ELEMENTS_TO_IMPROVE>; do
  SUBFOLDER="$WORK_FOLDER/improve_${INDEX}"
  mkdir -p "$SUBFOLDER"
  # All copies are best-effort -- if a snapshot step failed earlier, the missing file
  # will surface as a clearer error when the subagent tries to use it.
  cp "$WORK_FOLDER/ApplicationLevelNodeTreeInfo.json"        "$SUBFOLDER/" 2>/dev/null || true
  cp "$WORK_FOLDER/ApplicationLevelApplicationMetadata.json" "$SUBFOLDER/" 2>/dev/null || true
  cp "$WORK_FOLDER/ApplicationScreenshot.jpg"                "$SUBFOLDER/" 2>/dev/null || true
  cp "$WORK_FOLDER/TopLevelNodeTreeInfo.json"                "$SUBFOLDER/" 2>/dev/null || true
  cp "$WORK_FOLDER/TopLevelApplicationMetadata.json"         "$SUBFOLDER/" 2>/dev/null || true
done
```

The definition files stay in `$WORK_FOLDER` (each `target-${INDEX}.xaml` is already unique). The subagent updates its definition file in place, so TARGET-7/8/9 pick up improved selectors with no copy-back step.

**IMPORTANT**: Each agent must be a separate, self-contained `Agent` tool call. Use `model: "sonnet"`. Don't try to inline it.

Use the prompt below for each, with these substitutions:
- `$DEF_FILE` -> `$WORK_FOLDER/target-${INDEX}.xaml` (element mode) or `$WORK_FOLDER/window.xaml` (screen-only mode)
- `$AGENT_FOLDER` -> `$WORK_FOLDER/improve_${INDEX}` (element mode) or `$WORK_FOLDER` (screen-only mode)
- `$NODE_INFO` -> what is the purpose of this element, where is it located in the tree, what nodes should be present in the hierarchy
- `$ATTR_INFO` -> interesting attributes to include, volatile attributes to avoid, attributes that should be parametrized
---

You are improving UiPath selectors to make them more robust. Follow the instructions in the skill file mechanically.

Target element: $NODE_INFO
Attribute guidance: $ATTR_INFO

1. Read `../uia-improve-selector/SKILL.md` (relative to the directory this file is in) to learn the full procedure.
2. Execute the skill steps with these arguments: `$DEF_FILE --folder $AGENT_FOLDER --mode improve --quiet` (add `--project-dir $PROJECT_DIR` if `$PROJECT_DIR` is set).
3. The definition file contains the current selectors. Improve whatever is present -- window selector only or window + element selector together.

---

Wait for all agents to complete.

Re-assess each improved selector against the criteria above. If any selector is still not `RELIABLE` you must add the reason to `$ATTR_INFO` and run it again.

**Element mode only:** if the window selector was improved, read the `ScopeSelectorArgument` from the first improved element's definition and update `$WORK_FOLDER/window.xaml` with it (so TARGET-9 screen creation uses the improved window selector).

## TARGET-7: Configure Semantic Targeting

**Skip if `$CONFIGURE_SEMANTIC` is `false` or screen-only mode.** Proceed to TARGET-8.

For each element in `$ELEMENTS_TO_CREATE`:

Derive a natural-language description of the element from `$ELEMENT` (e.g., `"Submit button in the order form"`). Save as `$SEMANTIC_SELECTOR`.

Write the semantic selector into the element definition via the CLI (never edit the XAML directly):

```bash
cli target-anchorable update-definition \
  --definition-file-path "$WORK_FOLDER/target-${INDEX}.xaml" \
  --semantic-selector "$SEMANTIC_SELECTOR"
```

## TARGET-8: Configure Anchors

**Skip in screen-only mode.** Proceed to TARGET-9.

An **anchor** is a nearby, stably-identifiable element that disambiguates the main target when the target's own selector is not uniquely reliable. Find a good anchor by **reading the captured tree and reasoning about it**.

### 8.1 Gates and classification

**Screen-only mode:** skip the entire step -- the window/screen is never anchored.

For each element from `$ELEMENTS_TO_CREATE`, read the `SearchSteps` attribute from its `TargetAnchorable` in `$WORK_FOLDER/target-${INDEX}.xaml` and proceed accordingly, considering its selector search step:

- **`FuzzySelector`**: Read the definition file. If it has **no anchor registered**, add the element to `$FUZZY_ELEMENTS_TO_ANCHOR`; if it **already has an anchor**, skip it (it is already disambiguated — don't add a second).
- **strict `Selector`**: Add the element to `$STRICT_ELEMENTS_TO_ANCHOR`.

### 8.2 Decide whether an anchor is needed

For each element in `$STRICT_ELEMENTS_TO_ANCHOR`, read its `FullSelectorArgument` from `$WORK_FOLDER/target-${INDEX}.xaml`. Anchor an
element **only when its selector cannot identify it uniquely AND stably on its own.** Apply the rules below in order and **stop at the first
that matches**:

1. **Positional ⇒ KEEP.** The selector contains `idx`, `tableRow`, `tableCol`, or any other purely positional attribute, anywhere in any
tag. Positional attributes are never stable -- keep it. A label or content constraint elsewhere in the selector (e.g.
`visibleinnertext='*ETH gas*'`) does **NOT** offset a positional attribute: `idx` present ⇒ positionally dependent, full stop.

2. **Volatile-as-identifier ⇒ KEEP.** The element's identity rests on an attribute that reflects the target's **own changing content** --
`text`/`aaname`/`visibleinnertext`/`innertext` matching the value, `checked`/`aastate`, or `selecteditem`/`value`. Keep it.
    - **Exception (does NOT trigger keep):** a content attribute that matches a *stable label, column, or header used to scope a
parent/row/column* -- e.g. `colName='Market cap'`, or `visibleinnertext='*Solana*'` on an enclosing `TR` to pick a row. That is
scope-by-stable-label, not identify-by-volatile-value.

3. **Unpinned sibling ⇒ KEEP.** The element is one of several near-identical siblings (a cell in a grid, a button repeated per row/card)
**AND** the selector does not already pin the specific one via a stable row/column/section scope (per the Rule 2 exception). If the selector
already pins it (stable row scope + `colName`, a header, etc.), it is not unpinned -- do not keep on this basis.

4. **Otherwise ⇒ DROP.** None of 1-3 matched, so the selector identifies the element uniquely and stably on its own (a developer-assigned
`automationid`/`id`/`name`, or a stable row-scope + column-name combination). An anchor would only add fragility. **Remove it from
`$STRICT_ELEMENTS_TO_ANCHOR`.**

### 8.3 Find anchor candidates

Create the `$ELEMENTS_TO_ANCHOR` list containing every element from `$STRICT_ELEMENTS_TO_ANCHOR`, plus every element from `$FUZZY_ELEMENTS_TO_ANCHOR`. 8.4 wires both buckets identically.

For each element in the new `$ELEMENTS_TO_ANCHOR` list, search the tree for the target's surroundings, then keep the candidates that make good anchors.

**Search the tree.** The target's snapshot ref is `$EREF_${INDEX}` (saved in TARGET-5). Locate it in `tree.md` and inspect its surroundings -- never read the tree unbounded:

```
Grep pattern="\[ref=$EREF_${INDEX}\]" path="$WORK_FOLDER/tree.md" output_mode="content" -n=true
```

Use the matched line number with `Read` (`offset`/`limit`) to view the target's parent, siblings, and nearby leaf nodes (indentation encodes structure). If structural proximity is ambiguous, read `$WORK_FOLDER/ApplicationScreenshot.jpg` once to confirm a candidate sits visually next to the target.

**A good anchor is:**

- **Stable** -- carries distinctive, non-volatile text or a developer-assigned identifier: a field label, a column/row header, a section caption, or static descriptive text. Avoid anything that changes with content, state, or data (timestamps, counts, row values).
- **Close** -- a sibling, the label immediately adjacent to the target, or a node within 1-2 tree levels. The closer and more directly associated, the better.
- **Unique** -- its own identity is unambiguous in the tree. Do not anchor to one of many identical nodes.

Prefer in this order: an explicit label for the target (e.g., the static text immediately preceding an input), then the table header for a grid cell, then the nearest stable neighbor.

A target holds **up to four anchors** (slots `0..3`). One well-chosen anchor is usually enough -- prefer a single strong one. Select more only when no single anchor disambiguates the target on its own (e.g., a grid cell that needs both its column header and its row header). Don't pad the slots; each extra anchor must also be found at runtime, so a weak one adds fragility.

Pick the best candidate (or up to four). If no nearby element qualifies, **skip this element** -- a misleading anchor is worse than none.

### 8.4 Wire the anchors

For each element in `$ELEMENTS_TO_ANCHOR`, wire its chosen anchor(s) with `add-anchor` (never edit the XAML directly). **Wire one anchor at a time -- run steps 1-3 fully for one anchor before adding the next.** (`remove-anchor` in step 3 shifts the remaining slot indices down, so juggling several half-wired anchors at once makes a saved `$SLOT` go stale.)

For each anchor, save its tree ref as `$AREF` and a short Title-Case label as `$ANCHOR_NAME`, then **try the live selector first, falling back to the snapshot selector only if the live one is weak**:

1. **Add with the live selector** (no `--from-snapshot`). The call writes the anchor to the next free slot and returns its index -- save it as `$SLOT` (`0..3`):

   ```bash
   cli target-anchorable add-anchor \
     --folder-path "$WORK_FOLDER" \
     --ref "$AREF" \
     --definition-file-path "$WORK_FOLDER/target-${INDEX}.xaml" \
     --name "$ANCHOR_NAME"
   ```

2. **Assess the anchor's selector.** Read the anchor at slot `$SLOT` from `$WORK_FOLDER/target-${INDEX}.xaml`. An anchor resolves to **either** a strict or a fuzzy selector depending on the element (text-based anchors — labels, headers, static text — come out fuzzy), so first read the anchor's `SearchSteps` and assess whichever selector is enabled:
   - `Selector` (strict) → judge the `FullSelectorArgument`.
   - `FuzzySelector` (fuzzy) → judge the `FuzzySelectorArgument`.

   Judge it with the same reliability criteria as TARGET-6 (reliable identifying attributes, not positional, stable values, ~2 tags). If it is reliable, the anchor is done.

3. **If it is not reliable enough, retry from the snapshot.** Remove the anchor you just added, then re-add the same ref with `--from-snapshot` -- the snapshot tree can yield a more stable selector:

   ```bash
   cli target-anchorable remove-anchor \
     --definition-file-path "$WORK_FOLDER/target-${INDEX}.xaml" \
     --index "$SLOT"

   cli target-anchorable add-anchor \
     --folder-path "$WORK_FOLDER" \
     --ref "$AREF" \
     --definition-file-path "$WORK_FOLDER/target-${INDEX}.xaml" \
     --name "$ANCHOR_NAME" \
     --from-snapshot
   ```

   Re-assess the new anchor's enabled selector the same way as in step 2 (strict → `FullSelectorArgument`, fuzzy → `FuzzySelectorArgument`). If neither the live nor the snapshot selector is reliable, remove the anchor you just added (`remove-anchor --index "$SLOT"`), then pick a different anchor from 8.3 -- or skip the element.

## TARGET-9: Register in Object Repository

**If `$SCREEN_REF_ID` is empty** (no matching screen found in TARGET-3), create it. First, write the screen name and description into the definition file. Compose a description that captures what the screen represents in the application — include the app name, the purpose of the screen/page, and any distinguishing context (e.g., `"The main login page of the Acme HR portal"`, `"Calculator application main window"`):

```bash
cli target-app update-definition \
  --definition-file-path "$WORK_FOLDER/window.xaml" \
  --name "$SCREEN_NAME" \
  --description "$SCREEN_DESCRIPTION"
```

Then register:

```bash
cli object-repository create-screen --definition-file-path "$WORK_FOLDER/window.xaml"
```

Save stdout as `$SCREEN_REF_ID`. If the command fails, show the error and stop.

**Screen-only mode:** skip to **Output**.

**Update each element definition file with name and description** before registration. For each element in `$ELEMENTS_TO_CREATE`, compose a description that explains the element's role and location within the screen — include what type of control it is, what it does, and where it sits in the UI (e.g., `"The Submit Order button at the bottom of the checkout form"`, `"Username text input field in the login panel"`):

```bash
cli target-anchorable update-definition \
  --definition-file-path "$WORK_FOLDER/target-1.xaml" \
  --name "$ELEMENT_NAME_1" \
  --description "$ELEMENT_DESCRIPTION_1"
cli target-anchorable update-definition \
  --definition-file-path "$WORK_FOLDER/target-2.xaml" \
  --name "$ELEMENT_NAME_2" \
  --description "$ELEMENT_DESCRIPTION_2"
# ... one per element
```

**Create all elements in a single batched CLI call** using comma-separated definition file paths:

```bash
cli object-repository create-elements \
  --screen-reference-id "$SCREEN_REF_ID" \
  --definition-file-paths "$WORK_FOLDER/target-1.xaml,$WORK_FOLDER/target-2.xaml"
```

Each output line prints `$ELEMENT_NAME -> $ELEMENT_REF_ID` (or `FAILED: $ELEMENT_NAME (error)`). Parse the output to collect `{$ELEMENT_NAME, $ELEMENT_REF_ID, created}` for every element.

## Output

Present the results concisely: screen reference ID, element reference IDs (table if multiple). No observations, no quality notes, no suggestions.

**Important:** Follow the TARGET steps sequentially with discipline. If you get sidetracked by errors, retries, or user questions, always return to complete the remaining steps in the flow.
