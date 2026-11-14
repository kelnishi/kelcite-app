# Changelog Hotfix 0.17.98.5

Released 2026-11-09.

Deleting or baking a million-triangle body no longer ends the page on an **iPhone**.

## Bake

Wiki: [Bake an SDF to a Mesh](Workflow-Bake-an-SDF-to-a-Mesh)

- Opening Bake on a large body right after deleting another no longer ends the page on an iPhone: before it builds a preview of half a million triangles or more, Bake gives back the memory a deleted body and an idle mesh check still held, and a large preview gives back its graphics memory a moment after it shows.

## Rendering and performance

- Deleting or baking a body of a million triangles or more no longer ends the page on an iPhone: the delete effect flies apart at most 100,000 of its triangles, a deleted body's drawing gives back its graphics memory a few seconds later, and the mesh check after Bake sets aside the memory it needs in one step.
- The warning that the scene holds too much in graphics memory goes away once the scene is back under the budget, instead of naming bodies that were deleted.
