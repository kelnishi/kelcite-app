# Changelog Feature Release 0.9.4

Released 2026-10-14.

**Extrude** follows a pin and gains Inset, undo history survives a reload with its geometry, and touch screens drive gizmos with one finger.

## Modifiers

Wiki: [Modifiers](Tool-Modifiers)

- Extrude follows a pin, meets the face cleanly, and has Inset; a negative depth cuts a pocket

## Objects and the interface

- Opening a tool closes the open one first, and Skeleton opens on the selected body or on the body Clay just placed
- Undo history saved before a reload restores its geometry, and deleting a node clears the pins, constraints and setup bindings that pointed at it
- The app shows when it is ready to work offline, and touch screens drive gizmos and handles with one finger
