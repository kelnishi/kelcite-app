# Changelog Feature Release 0.9.6

Released 2026-10-14.

New hair walls are **offsets** that stand on the build plate, hair can aim for a strand count, and a Materials row can merge or separate materials.

## Modifiers

Wiki: [Modifiers](Tool-Modifiers)

- New hair walls are offsets: a smooth wall a set distance out from where the strands leave, standing on the build plate, so strands meet it square; a hull wall stands straight up around the whole body. Both flare their base out to hold to the plate
- Hair can aim for a strand count, and hair on a face of a few large triangles places strands
- Hair recomputes faster, with a spinner in the Objects panel while it does, and Paint shows hair strands as lines so they don't hide the faces being painted

## Objects and the interface

- Each row of a mesh's Materials list can merge the material into another, or separate its disjoint regions into materials of their own
