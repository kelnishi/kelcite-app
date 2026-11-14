# Changelog 0.15.24

Released 2026-10-27.

Tools **remember their options**, Preferences gains a **Data** tab, Bake can aim for a triangle count, and Crack gains an Advanced section and opens large meshes quickly.

## Modifiers

Wiki: [Modifiers](Tool-Modifiers)

- A new hair wall starts as a Hull wall, standing a set distance outside the whole body seen from above, where it started as an Offset wall
- Hair remembers the settings you last chose: a new hair emitter, wall or field starts from them
- Crossing strands has a Minimum gap: strands in one layer closer than this are flagged, since they would likely fuse in the slicer; new hair keeps one extrusion width between strands
- Strain relief has a Buried depth: sinking the cone into the surface anchors the strand inside the model, and cones that break through the far side are flagged
- 2-lane strands: each hair strand prints as two side-by-side lanes, twice the width, so the slicer bridges it in both directions; it is on for new hair, which spaces its strands wider to keep them apart
- Strain relief says when its radius is too small to build a cone
- Each section of the hair settings has an info button that explains what it is for
- Each hair placement strategy shows a picture of its strand layout and what it suits, when you point at its tab
- Hair's Density slider changes strand spacing evenly along its whole range, with finer control at low densities, and new hair starts at half the density it did
- Hair strands rooted closer together than their width are flagged, where those that touched near the surface went unmarked, and Grid placement thins out where strands would crowd, near a dome's top or a cone's tip

## Bake

Wiki: [Bake an SDF to a Mesh](Workflow-Bake-an-SDF-to-a-Mesh)

- Bake's new size options and messages are translated
- Bake's Cell size is the cell the mesh is made at, where the mesh used a finer cell up to half its size; at the same setting a mesh now has fewer triangles, so set a smaller cell for the old detail
- The baked mesh is the one previewed, where Split divided its face groups a second time
- Switching between Cell size and Target triangles keeps the mesh, and leaving a reached target keeps the cell it found
- The cell slider starts at the finest cell the method can reach at this size, and says so
- The chosen Method and Size in Bake show in blue, as do the Pose bake's Color and Method
- Bake's SDF → Mesh can aim for a triangle count instead of a cell size: it finds the cell size that gives the most triangles at or under the target, and shows the count and cell size it reached

## Split and Combine

Wiki: [Split and Cut a Model](Workflow-Split-and-Cut-a-Model)

- Split on a mesh starts from the selection's center, where it could reuse a snap point from an earlier session

## Jig

Wiki: [Construction and Rigging](Tool-Construction-and-Rigging#jig)

- Jig's stiffness from the Setup Table reaches the constraints it makes, where every one was made at full stiffness

## Repair

Wiki: [Repair a Scan for Printing](Workflow-Repair-a-Scan-for-Printing)

- The reset page also clears the Repair tool's saved options

## Crack and Scribe

Wiki: [Surface and Refine](Tool-Surface-and-Refine)

- Crack's Part mode leaves the remaining piece as one closed body with matching caps, where it printed a thin slab per group with walls inside
- After Refine or Subdivide, every crack follows the new mesh; a crack through the changed area is removed with a notice
- A curved cap that has to stay flat in places says so
- Crack has an Advanced section: Merge small regions with a Minimum region, Keep existing groups, Refine smoothing and strength, and a Curved cap in Part mode
- Crack's group colors stay put while the angle changes, crack lines stay visible over any group color, and a note appears when an angle leaves a thousand groups or more
- Dragging Crack's angle shows the crease lines and groups as you drag, and arrow keys step it; opening Crack no longer blocks the page while it studies the mesh
- Crack lines use the construction colors, and long group lists stay quick
- Opening Crack on a large mesh no longer freezes the page: a 223k-triangle mesh is ready in about 0.2 s instead of 8.6 s, and hovering no longer stutters

## Objects and the interface

- A notification that asks you to fix something links to where you fix it: no filament profiles opens Setup at Filament Profiles, a mesh that isn't manifold links to Repair, and Bind and Bake reminders link to their tools
- Selected toggles across the panels are a deeper blue, so their white labels read clearly
- Opening Preferences closes the keyboard cheatsheet, which sat where the panel opens
- A notification about an object names it as a link: clicking selects the object and frames it, and hovering highlights it
- Tools reopen in the mode you left them in, such as Move's translate, rotate or scale, unless you turn off Remember tool mode in Preferences → Data
- An option in Preferences → Data opens every tool on factory settings instead of your last choices
- Preferences has a Data tab: export and import your keybindings, tool settings and preferences as one file, reset any tool's settings, see storage use, and Reset everything
- A settings file from a newer Kelcite imports what this version reads and names the rest; keybinding import now handles renamed commands
- Split, Move, Pin, Scribe, Crack, Trace and Bake open on the options last chosen; new traces and each kind of Bake operation start from the last ones set
- Clay, Text, Refine, Repair and Paint keep their options between visits: Text opens with the style last used, and Repair no longer switches back to its default strategy
- Refine's tolerance and Paint's brush size are distances, so they read the same in any unit
- Skeletonize, Combine, Jig, Rig and Pose open on the options you last chose; Skeletonize scales remembered sizes to each body
