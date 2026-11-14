# Changelog Feature Release 0.9.1

Released 2026-10-11.

**Printable hair**: bridged one-layer strands from a region of the mesh to a support wall, baked into one closed solid that a slicer prints without a filament swap.

## Modifiers

Wiki: [Modifiers](Tool-Modifiers)

- Tapered fins lift strands off shallow faces, and support walls have sectors, keep-outs and a fitted lean
- Baked and exported hair is one closed solid in the surrounding material, so slicers print it without a filament swap or non-manifold edges
- A Validation angle marks strands on faces that point at the build plate, and Move, modifier and hair settings undo one step at a time

### Hair

- Printable, bridged hair, one-layer strands from a region of the mesh to a support wall, placed by grid, stratified, Poisson or cellular placement, with edge fade, emission fields and jitter
