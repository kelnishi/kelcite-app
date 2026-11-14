# Changelog 0.17.98.3

Released 2026-11-06.

Zooming no longer leaves graphics memory behind with shadows on.

## Rendering and performance

### Viewport

- Zooming in and out no longer leaves graphics memory behind with shadows on; before, each zoom past half a meter from the origin held on to about 32 MB until the page reloaded, and the app slowed as it piled up.
