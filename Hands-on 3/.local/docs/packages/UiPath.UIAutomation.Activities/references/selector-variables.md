# Selector Variables

How to inject variable/argument values into selector text. The syntax depends on where the selector is stored:

- In an **XAML** → use `string.Format` positional placeholders inside the InArguments that accept a selector.
- In a **definition file** → use the `{{variableName}}` delimiter syntax.

Both forms resolve to the same selector at runtime.

> **Important:** Every variable referenced from a selector — in either form — must be declared in the XAML, either as an argument or as a variable on an enclosing activity (for example, the parent `Sequence`). The runtime resolves variables against the activity context; tokens that cannot be resolved fall back to literal text.

> **Definition files are produced and consumed only by the [`uia-configure-target`](../skills/uia-configure-target/SKILL.md) skill** (`window.xaml`, `target-n.xaml`). Outside of that flow — i.e. when authoring or editing an XAML directly — selector variables always live in an InArgument and must use the `string.Format` form.

## In XAML

The InArguments that carry selector text and support variable interpolation are:

- `Selector` — selector on a `TargetApp` (Use Application/Browser). This is the only property on `TargetApp` that supports variables.
- `ScopeSelectorArgument` — window selector on a `TargetAnchorable`.
- `FullSelectorArgument` — strict element selector on a `TargetAnchorable`.
- `FuzzySelectorArgument` — fuzzy element selector on a `TargetAnchorable`.

When the selector value is provided through one of these `InArgument<string>`s, the expression must follow `string.Format` rules:

- Positional placeholders: `{0}`, `{1}`, …
- Each placeholder is bound, in order, to the variables listed after the format string.
- Literal `{` and `}` must be escaped as `{{` and `}}`.

VB expression:

```text
String.Format("<webctrl id='{0}' tag='{1}' />", elementId, tagName)
```

C# expression:

```text
string.Format("<webctrl id='{0}' tag='{1}' />", elementId, tagName)
```

A single-variable selector is also accepted as the bare variable name (no `string.Format` wrapper):

```text
mySelector
```

### XAML example

The formatted expression goes into `ScopeSelectorArgument` / `FullSelectorArgument` / `FuzzySelectorArgument` on a `TargetAnchorable`:

```xml
<uix:TargetAnchorable Version="V6">
  <uix:TargetAnchorable.ScopeSelectorArgument>
    <InArgument x:TypeArguments="x:String">[string.Format("&lt;wnd app='chrome.exe' title='{0} - Google Chrome' /&gt;", pageTitle)]</InArgument>
  </uix:TargetAnchorable.ScopeSelectorArgument>
  <uix:TargetAnchorable.FullSelectorArgument>
    <InArgument x:TypeArguments="x:String">[string.Format("&lt;webctrl id='{0}' tag='BUTTON' /&gt;", elementId)]</InArgument>
  </uix:TargetAnchorable.FullSelectorArgument>
  <uix:TargetAnchorable.FuzzySelectorArgument>
    <InArgument x:TypeArguments="x:String">[string.Format("&lt;webctrl id='{0}' /&gt;", elementId)]</InArgument>
  </uix:TargetAnchorable.FuzzySelectorArgument>
</uix:TargetAnchorable>
```

For a `TargetApp`, the same applies to `Selector` (the only `TargetApp` property that supports variables):

```xml
<uix:TargetApp Version="V2">
  <uix:TargetApp.Selector>
    <InArgument x:TypeArguments="x:String">[string.Format("&lt;wnd app='chrome.exe' title='{0} - Google Chrome' /&gt;", pageTitle)]</InArgument>
  </uix:TargetApp.Selector>
</uix:TargetApp>
```

## In definition files

When the selector text is stored as a literal string in a definition file, variables are written inline using double-brace delimiters:

```text
<webctrl id='{{elementId}}' tag='{{tagName}}' />
```

Rules:
- The token between `{{` and `}}` must be a valid identifier; otherwise it is treated as literal text.
- The same variable can appear multiple times — every occurrence is substituted with the same resolved value.
- Resolution at runtime pulls values from the activity context (XAML arguments / variables).

### Definition file examples

A definition is a XAML file plus a metadata file (`*.xaml.metadata`) that share the same base name. The XAML holds the serialized `TargetApp` / `TargetAnchorable` with `{{variable}}` tokens inside the `InArgument` selector text; the metadata file holds design-time metadata only.

`window.xaml` — the serialized `TargetApp` produced by `uia-configure-target` for the screen:

```xml
<uix:TargetApp Version="V2" Area="0, 0, 0, 0">
  <uix:TargetApp.Selector>
    <InArgument x:TypeArguments="x:String">&lt;wnd app='chrome.exe' title='{{pageTitle}} - Google Chrome' /&gt;</InArgument>
  </uix:TargetApp.Selector>
</uix:TargetApp>
```

`window.xaml.metadata` — the metadata file (`Name`, `Description`, `WindowNodeId`):

```json
{
  "Name": "Google Chrome",
  "Description": "Google Chrome window",
  "WindowNodeId": "w3"
}
```

`target-1.xaml` — the serialized `TargetAnchorable` for a per-element definition. `ScopeSelectorArgument` is seeded from the window's `Selector`, and `FullSelectorArgument` is the full element selector (scope + partial):

```xml
<uix:TargetAnchorable Version="V6" DesignTimeRectangle="0, 0, 0, 0" Guid="11111111-1111-1111-1111-111111111111">
  <uix:TargetAnchorable.ScopeSelectorArgument>
    <InArgument x:TypeArguments="x:String">&lt;wnd app='chrome.exe' title='{{pageTitle}} - Google Chrome' /&gt;</InArgument>
  </uix:TargetAnchorable.ScopeSelectorArgument>
  <uix:TargetAnchorable.FullSelectorArgument>
    <InArgument x:TypeArguments="x:String">&lt;wnd app='chrome.exe' title='{{pageTitle}} - Google Chrome' /&gt;&lt;webctrl id='{{elementId}}' tag='BUTTON' /&gt;</InArgument>
  </uix:TargetAnchorable.FullSelectorArgument>
  <uix:TargetAnchorable.SemanticSelectorArgument>
    <InArgument x:TypeArguments="x:String">Submit button labeled {{elementId}}</InArgument>
  </uix:TargetAnchorable.SemanticSelectorArgument>
</uix:TargetAnchorable>
```

`target-1.xaml.metadata` — the metadata file (`Name`, `Description`, `WindowNodeId`, `ElementNodeId`, `ActivityType`):

```json
{
  "Name": "Submit Button",
  "Description": "The Submit button in the form",
  "WindowNodeId": "w3",
  "ElementNodeId": "e42",
  "ActivityType": "Click"
}
```

When the OR registers these definitions, the `{{pageTitle}}` and `{{elementId}}` tokens inside the `InArgument` selector text are converted to the corresponding `string.Format` expressions on the activity that consumes them.

## Conversion between the two forms

| Direction | Example |
|-----------|---------|
| Definition → XAML | `<webctrl id='{{x}}' />` → `string.Format("<webctrl id='{0}' />", x)` |
| XAML → Definition | `string.Format("<webctrl id='{0}' />", x)` → `<webctrl id='{{x}}' />` |
