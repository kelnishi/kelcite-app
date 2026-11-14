# Changelog Bug Fix 0.12.28

Released 2026-10-19.

Transparent pixels in a displacement map leave the surface where it is.

## Clay

Wiki: [Sculpt with Clay](Workflow-Sculpt-with-Clay)

- A transparent pixel in a displacement map leaves the surface where it is, on clay and meshes, and a half-transparent one moves it halfway; it read as black and sank the surface
