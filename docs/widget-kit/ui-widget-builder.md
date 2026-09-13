# WidgetKit UI widget builder

This document describes a UI-first workflow for creating widget shells without live data.

## Idea

The user should be able to build a widget from the outside in:

- pick an area
- set title and description
- choose layout size
- add links and badges
- preview the shell immediately

The dynamic data adapter comes later. For now the builder focuses on structure and presentation.

## Builder model

The builder edits a simple structure that maps cleanly to the reusable core:

- `widget_id`
- `label`
- `description`
- `area`
- `category`
- `width`
- `height`
- `links`
- `stats`
- `rows`

This is enough to define most widget shells and preview their appearance.

In a host project, this builder state can later be mapped to `WidgetSpec` plus a normalized payload preview.

## UI layout

The prototype uses three panes:

1. **Library** — reusable widget presets
2. **Editor** — the selected widget shell
3. **Preview** — the rendered result

## Interaction

- selecting a preset loads its shell
- editing fields updates the preview
- links and rows can be added or removed
- size changes are reflected immediately

## Follow-up

The next step is persistence: saving widget shell definitions per area and reusing them in a project-level dashboard or page builder. WidgetKit itself should remain the transportable core, while storage and UI orchestration belong to the consuming application.
