# Changelog 0.15.25

Released 2026-10-27.

Right-click a face group to **extrude** it or add a decal, search adds modifiers, and Extrude gains Along Average Normal.

## Modifiers

Wiki: [Modifiers](Tool-Modifiers)

- Right-click a face group, on the mesh or in the Materials list, to extrude it or to add a Decal or Texture Map limited to it
- Search adds modifiers to the selected mesh: Extrude, Boolean, Decal, Texture Map, Hair and Lattice
- Extrude's new Along Average Normal direction raises a patch straight off its surface, without placing a pin
- Extrude's Depth and Inset sliders scale to the faces being extruded, and reset to zero
- Extruding along normals no longer folds through itself on curved surfaces: a cut deeper than the surface's curve stops evenly short of it
- An extrusion's walls get a face group of their own, and baking gives them a material named after the top's
- A mesh with modifiers shows its painted colors and face groups in the viewport
- Dragging Extrude's sliders updates the preview several times faster
- While you paint, modifiers that read face groups follow the stroke. Ghost Opacity sets how visible their result is, and holding Shift hides it

## Trace

Wiki: [Construction and Rigging](Tool-Construction-and-Rigging#trace)

- Trace's Split mesh and Re-paint options work on Apply as the trace's own menu items do, and Apply reuses the preview instead of computing it again
