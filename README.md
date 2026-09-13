# WidgetKit

WidgetKit is a framework-neutral Python package for defining, shaping, and rendering reusable UI widgets. It is designed as a portable foundation for a flexible widget builder and GUI configurator that can be embedded into other web projects.

The package keeps three concerns separate:

1. **Widget definition** via structured metadata
2. **Widget data shaping** via host-app adapters
3. **Widget rendering** via Jinja templates

This separation makes the library suitable both for direct server-side rendering and for future builder workflows where widget shells, presets, previews, and configuration UIs need a stable underlying model.

## Current scope

The repository currently provides:

- a small widget domain model in `widgetkit/core.py`
- an adapter registry in `widgetkit/registry.py`
- a Jinja-based renderer in `widgetkit/renderer.py`
- a thin integration facade in `widgetkit/integration.py`
- basic HTML templates for card-style widget previews

## Design goals

- **Reusable across projects**: no Django dependency, minimal runtime surface
- **Builder-friendly**: metadata and payloads stay explicit and structured
- **Composable**: host applications own data loading and integration details
- **Predictable rendering**: templates receive normalized widget payloads

## Architecture

### 1. Widget metadata

`WidgetSpec` describes a widget independently from any concrete application:

- identity (`widget_id`)
- placement (`area`, `category`)
- presentation defaults (`label`, `description`, sizing)
- rendering hints (`render_kind`, `preview_kind`)
- configuration fields (`config_schema`)
- adapter binding (`data_adapter`)

### 2. Widget payload

Adapters return a generic payload structure used by the renderer and by potential builder UIs:

- `widget_id`
- `label`
- `description`
- `stats`
- `rows`
- `links`
- optional `body`

`empty_payload()` creates a minimal payload shell, and `payload_context()` normalizes payloads into a render context.

### 3. Rendering

`render_card()` renders a widget payload with `card.html`. `render_fragment()` renders named template fragments for more granular use cases. Template lookup is configurable through `WidgetKitConfig`.

## Example

```python
from widgetkit import empty_payload, render_card

payload = empty_payload("example.card", "Example card", "Minimal widget")
payload["stats"] = [{"label": "Status", "value": "ok"}]
payload["rows"] = [{"label": "Source", "value": "demo"}]
payload["links"] = [{"label": "Open", "url": "/example"}]

html = render_card(payload)
```

## Intended use in host applications

WidgetKit is intended to sit below a project-specific dashboard, page builder, or configuration UI:

1. The host app declares available widget types with `WidgetSpec`.
2. The host app registers adapters that map app state into generic widget payloads.
3. Builder or admin UIs edit metadata and shell configuration.
4. The renderer produces previews or final HTML output from the normalized payload.

This makes the package suitable as a shared widget core while each consuming web project stays free to choose its own persistence, routing, permissions, and frontend technology.

## Repository layout

- `widgetkit/` — package source
- `widgetkit/templates/` — Jinja templates
- `docs/widget-kit/` — concept and implementation notes
- `tests/` — lightweight smoke coverage

## Documentation

- `docs/widget-kit/concept.md` explains the conceptual model
- `docs/widget-kit/implementation.md` describes the current package structure
- `docs/widget-kit/ui-widget-builder.md` outlines the builder-oriented direction

## Status

The current codebase is a compact core, not yet a full widget builder. It is already shaped to serve as the reusable backend layer for one: portable widget specs, adapter-driven payload creation, and renderer-backed previews are in place.
