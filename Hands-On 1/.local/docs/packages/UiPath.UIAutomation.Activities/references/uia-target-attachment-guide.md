# OR Target Attachment

How to attach Object Repository screens and elements to XAML workflow activities.

> **Inline JSON & Windows paths:** the `--elements` / `--targets` flags below take a JSON array. Use forward slashes (`C:/Users/...`) inside the JSON — Windows backslashes are interpreted as JSON escape sequences and the CLI will reject the value with a `Bad JSON escape` error.

## IdRef Contract

The linker addresses activities by `sap2010:WorkflowViewState.IdRef`. Every activity that will be linked MUST carry a unique IdRef of the form `<ClassName>_<N>` — for example `NApplicationCard_1`, `NClick_1`, `NClick_2`, `NTypeInto_1`. Numbering is per class and unique across the whole file. When inserting activities into an existing file, scan for the highest existing `<ClassName>_<N>` and continue from `N+1`.

This matches Studio's own naming convention, so files remain clean when re-opened in Studio.

## Fast Path: Linking OR Entries to Activities

Write plain activities (no `.Target` child element, or nested variants like `.SearchedElement.Target` for activities with nested targets) with unique IdRefs, then attach targets post-write using `link-screen` and `link-elements`

> **Do not run `link-screen` and `link-elements` in parallel.** These commands mutate the same `.xaml` file and will corrupt each other when invoked as parallel Bash tool calls. Link the screen first, then link all element targets with one `link-elements` call.

### 1. Link a screen to an ApplicationCard

```bash
uip rpa uia object-repository link-screen \
  --workflow-file-path "<RELATIVE_XAML_PATH>" \
  --activity-id "<ACTIVITY_REF_ID>" \
  --reference-id "<SCREEN_REFERENCE_ID>" \
  --project-dir "<PROJECT_DIR>" \
  --output json
```

| Flag | Required | Description |
|------|----------|-------------|
| `--workflow-file-path` | Yes | Path to the `.xaml` file, relative to the project directory (e.g., `Workflows/Main.xaml`). |
| `--activity-id` | Yes | The `sap2010:WorkflowViewState.IdRef` on the target activity — typically `NApplicationCard_1`. |
| `--reference-id` | Yes | OR screen reference returned by `uia-configure-target` or `indicate-application`. |

### 2. Link elements to UI activities

Use one batch call for all `(activity, element)` pairs in the workflow. `--elements` is a JSON array — one object per `(activity, element)` pair, each carrying its own `workflowFilePath` and an optional `targetProperty`.

```bash
uip rpa uia object-repository link-elements \
  --elements '[{"workflowFilePath":"<RELATIVE_XAML_PATH>","activityId":"<ACTIVITY_REF_ID_1>","referenceId":"<ELEMENT_REFERENCE_ID_1>"},{"workflowFilePath":"<RELATIVE_XAML_PATH>","activityId":"<ACTIVITY_REF_ID_2>","referenceId":"<ELEMENT_REFERENCE_ID_2>","targetProperty":"SearchedElement.Target"}]' \
  --project-dir "<PROJECT_DIR>" \
  --output json
```

| Flag | Required | Description |
|------|----------|-------------|
| `--elements` | Yes | JSON array of entries. Each entry: `workflowFilePath` is the path to the `.xaml` file (relative to the project directory or absolute inside it); `activityId` is the `sap2010:WorkflowViewState.IdRef` on the target activity (e.g., `NClick_3`); `referenceId` is the OR element reference returned by `uia-configure-target` or `indicate-element`; `targetProperty` is optional and defaults to `"Target"` (use dotted paths for nested properties, e.g., `"SearchedElement.Target"`). |

**When to set `targetProperty`:** most UI activities (`NClick`, `NTypeInto`, `NGetText`) attach the target at `.Target`, so the default is correct. Some activities expose the target at a nested property — e.g., the `NMouseScroll` activity has a nested target and uses `SearchedElement.Target`. When the target sits anywhere other than `.Target`, set `targetProperty` on that entry.

**Element reuse:** when the same element is referenced by multiple activities (e.g., the same field clicked and then typed into), include one JSON entry per activity, repeating the `referenceId` in each entry.

## Fallback: Embedding OR Entries When Linking Fails

Use this path only when a `link-screen` or `link-elements` call returns a non-zero exit or an error for a specific reference ID. Embed the OR XAML snippet directly into the matching activity — scoped to only the failed reference, not the whole screen.

### 1. Get the screen XAML for the ApplicationCard

```bash
uip rpa uia object-repository get-screen-xaml \
  --reference-id "<SCREEN_REFERENCE_ID>" \
  --project-dir "<PROJECT_DIR>"
```

Returns a `<TargetApp>` element. Embed it inside the ApplicationCard:

```xml
<uix:NApplicationCard.TargetApp>
  <uix:TargetApp .../>
</uix:NApplicationCard.TargetApp>
```

### 2. Get element XAML for UI activities

```bash
uip rpa uia object-repository get-elements-xaml \
  --reference-ids "<REF_1>,<REF_2>,<REF_3>" \
  --project-dir "<PROJECT_DIR>"
```

Returns `<TargetAnchorable>` elements, one per reference ID, separated by `=== Element Name ===` headers. Embed each inside its activity's `.Target` property (or the nested property named on the activity, e.g., `SearchedElement.Target`):

```xml
<uix:NClick ...>
  <uix:NClick.Target>
    <uix:TargetAnchorable .../>
  </uix:NClick.Target>
</uix:NClick>

<uix:NTypeInto ...>
  <uix:NTypeInto.Target>
    <uix:TargetAnchorable .../>
  </uix:NTypeInto.Target>
</uix:NTypeInto>

<uix:NGetText ...>
  <uix:NGetText.Target>
    <uix:TargetAnchorable .../>
  </uix:NGetText.Target>
</uix:NGetText>
```

| Parameter | Source |
|-----------|--------|
| `<SCREEN_REFERENCE_ID>` | OR screen reference returned by `uia-configure-target` or `indicate-application` |
| `<REF_1>,<REF_2>,...` | Comma-separated OR element references returned by `uia-configure-target` or `indicate-element` |

When an element is used by multiple activities (e.g., the same field clicked and then typed into), use the same `<TargetAnchorable>` snippet in each activity's `.Target` property.
