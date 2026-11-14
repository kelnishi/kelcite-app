# Changelog Hotfix 0.17.98.6

Released 2026-11-10.

**Pop out to window** works again, and background work restarts on browsers that had used an earlier version.

## Objects and the interface

### Panels

- Pop out to window opens the panel in its own window again. On browsers that had used Kelcite before version 0.17.98, the window stayed blank; this update repairs that without clearing any documents.

## Rendering and performance

- Work that runs in the background, such as mesh analysis, package downloads and 3MF export, starts again on those browsers, where it had stopped at once with "the worker stopped". The first load after this update downloads the app's files again, about 29 MB.
