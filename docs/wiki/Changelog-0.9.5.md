# Changelog Feature Release 0.9.5

Released 2026-10-14.

A skeleton can be limited to **region bodies**, and baking keeps the painted materials of skeletons and booleans.

## Modifiers

Wiki: [Modifiers](Tool-Modifiers)

- A skeleton can be limited to region bodies, mesh or clay: only the part inside them becomes lattice, and Invert swaps the two
- Opening Skeleton on several bodies gives the first the skeleton and makes the rest its regions, which Applying can tuck away and hide
- Baking a skeleton keeps the solid part's materials and prints the lattice in a material you choose
- Baking or exporting a boolean keeps the target's painted materials
