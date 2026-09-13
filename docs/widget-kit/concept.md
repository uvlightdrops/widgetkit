# WidgetKit concept

WidgetKit models a widget as a small, declarative unit with three concerns:

1. **Metadata** — identity, area, category, label, layout defaults
2. **Data** — a payload produced by an adapter
3. **Rendering** — a template that turns payload into HTML

The key idea is that widgets should not embed app logic inside templates. App-specific code belongs in adapters; the renderer only knows about generic payload fields.

## Separation of concerns

- **Core**: portable dataclasses and helper functions
- **Registry**: adapter lookup by string key
- **Renderer**: Jinja rendering with a configurable template directory
- **Integration**: host-app registration surface

## Universal integration principle

The package is meant to be reused in different web applications, so it avoids assumptions about:

- persistence
- authentication and permissions
- routing
- page composition
- JavaScript framework choices

Those concerns stay in the host project. WidgetKit supplies the common widget model and rendering contract.

## Why this shape

This keeps the library reusable outside Django and makes widget rendering predictable. It also makes it easy to build tools around widgets, because metadata and payloads stay structured.

## Future direction

The next step is a UI-first widget builder that lets users create the structural shell of a widget without worrying about live data yet. In that model, WidgetKit remains the backend-neutral widget core while consuming applications decide how the builder UI is exposed and saved.
