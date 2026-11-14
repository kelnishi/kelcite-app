# Changelog Feature Release 0.9.3

Released 2026-10-12.

Hair grows from a **material** picked on the mesh, lands on several walls and steers around other meshes, with crossing strands flagged or fixed.

## Modifiers

Wiki: [Modifiers](Tool-Modifiers)

- Hair grows from a material picked on the mesh, instead of a tagged region; saved documents convert when opened
- A hair modifier can land its strands on several walls, and strands steer around other meshes on their way
- Crossing strands are flagged, fitted onto the wall with the least turn that clears them, or pruned; clicking a strand turns it off or back on
- Fan out and Segment length shape the strands, and hovering a material lights the faces it covers
