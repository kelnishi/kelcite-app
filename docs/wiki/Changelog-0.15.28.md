# Changelog 0.15.28

Released 2026-10-28.

Extrude's sliders keep up on large meshes, and its **wall groups** show on its card.

## Modifiers

Wiki: [Modifiers](Tool-Modifiers)

- Dragging an Extrude's Depth, Inset or Direction redraws only the extruded patch, so the sliders keep up on large meshes
- Adjusting one of several Extrudes no longer rebuilds those before it, or those on other face groups, and an Extrude over another one's result builds without a boolean
- Extruding several face groups at once keeps each top face in the group it rose from, and the walls along each group take a wall group of their own
- An Extrude's wall groups show on its card and in the Materials section, marked Derived: change their color, merge them into another material, or point hair at them, which grows once the walls are baked
- Painted face groups around an extrusion show their colors again, and the extruded faces get a wireframe
