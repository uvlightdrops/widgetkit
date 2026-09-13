# WidgetKit implementation

The current implementation is split into small modules:

- `core.py` — dataclasses and payload helpers
- `registry.py` — in-memory adapter registry
- `renderer.py` — Jinja rendering
- `integration.py` — public registration helpers
- `templates/` — card and fragment templates

## Data flow

1. A host app defines a `WidgetSpec`.
2. The host registers an adapter under a key.
3. The adapter returns a structured payload.
4. The renderer converts the payload into HTML.

## Consistency notes

The current implementation is logically consistent with the intended direction as a reusable widget core:

- framework coupling is avoided
- adapter registration is explicit
- render input is normalized through `payload_context()`
- template lookup is configurable for host applications

What is not implemented yet is the higher-level builder stack itself, such as persistence, schema-driven editing, project-level widget catalogs, and frontend configurator flows. Those are correctly left outside the current core package.

## Notes

The implementation is intentionally minimal. It avoids framework coupling and keeps template lookup local to the package.
