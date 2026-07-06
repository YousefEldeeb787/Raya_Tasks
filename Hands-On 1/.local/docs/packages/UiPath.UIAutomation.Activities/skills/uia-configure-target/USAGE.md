# uia-configure-target — invocation

The skill is invoked by an agent to ensure a screen (and optionally one or more elements) exist in the Object Repository. It returns the OR reference ID(s) for workflow attachment.

## Invocation modes

- **TargetAnchorable** (element within a window — Click, TypeInto, GetText, etc.):

  ```
  --window <description> --elements <description>
  ```

- **TargetApp** (window only — Use Application/Browser):

  ```
  --window <description>
  ```

## Batch element configuration

Separate multiple element descriptions with `|` in a single `--elements` value to capture the window once and reuse it for all elements:

```
--window <description> --elements "element one | element two | element three"
```

Batch invocation avoids redundant window captures and screen lookups when multiple elements live on the same screen.

## What the skill does

Searches the Object Repository for existing matches before creating new entries, generates selectors from the live application tree, optionally improves and anchors them, and registers everything in the OR. After completion, the skill returns the reference ID(s) — one per element, plus the screen reference.

## Unsupported activities

This skill does not configure targets for the following activities:

- **UI Automation.SAP**: Call Transaction, SAP Login, Read Status Bar, Click Toolbar Button, Select Menu Item, Expand Tree, Table Cell Scope, Click Picture on Screen, Select Dates In Calendar, Expand ALV Tree, Expand ALV Hierarchical Table, SAP Logon
- **UI Automation.Semantic**: Fill Form, Update UI Element, Close Popup, Extract Form Data, Extract UI Data
- **Extract Table Data**

## Full argument reference

`SKILL.md` (sibling file) documents every argument with defaults and valid values, including `--semantic`, `--no-improve`, `--activity`, and `--project-dir`.
