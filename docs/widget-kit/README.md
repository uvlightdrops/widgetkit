# WidgetKit

WidgetKit is a small framework-neutral widget core. It separates widget metadata, adapter registration, payload shaping, and HTML rendering so the same widget model can be reused across multiple web projects.

## Goal

The goal is to keep the core tiny and reusable:
- stable public API
- simple widget metadata model
- adapter registry for app-specific data
- Jinja-based rendering outside any specific web framework
- thin integration layers for host apps
- a clean base for a future widget builder and GUI configurator

## Current implementation

### Core

`widgetkit/core.py` defines:
- `WidgetSpec` for widget metadata
- `WidgetContext` for rendering data
- `WidgetKitConfig` for renderer config
- `empty_payload()` and `payload_context()` helpers

### Registry

`widgetkit/registry.py` stores adapter functions by key. Host apps register adapters once during import/startup and resolve them by key later.

### Renderer

`widgetkit/renderer.py` creates a Jinja environment and renders:
- `card.html`
- fragment templates such as `stats.html`

### Integration

`widgetkit/integration.py` is the public facade for app-level registration. It exists so consuming apps do not need to know about the internal registry module.

## Universal usage model

WidgetKit should be treated as infrastructure, not as a project-specific dashboard implementation. A consuming application is expected to own:

- where widget definitions are stored
- how builder state is persisted
- which users may configure widgets
- where data comes from
- how rendered widgets are embedded into pages

WidgetKit provides the shared language between those layers: widget specs, normalized payloads, adapter resolution, and rendering.

## Packaging

The package is intentionally dependency-light. Jinja2 is the only rendering dependency.

## Example use

```python
from widgetkit import empty_payload, render_card

payload = empty_payload("example.card", "Example card", "Minimal widget")
payload["rows"] = [{"label": "Status", "value": "ok"}]
html = render_card(payload)
```

## Direction

The broader target is a flexible widget builder for other web applications. The package is therefore intentionally structured so that a future GUI configurator can edit widget shells and preview them without depending on any one host framework.

## Repository layout

- `widgetkit/`
- `widgetkit/templates/`
- `tests/`
- `README.md`
