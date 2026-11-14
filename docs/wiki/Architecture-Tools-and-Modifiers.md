# Architecture: Tools and Modifiers

Tools are how you change a Kelcite scene. Some produce a finished result directly; others leave [modifiers](Architecture-Scene-Model) that you can keep adjusting. That difference explains why a tool may ask you to Bake first, and why a result can appear in the Modifiers panel instead of in the mesh.

## How a tool works

A tool is the unit of interaction in Kelcite. Activating one gives it the inspector panel, its own key bindings and, often, gizmos in the 3D view. Most tools preview their change while you adjust it, and apply it when you commit with Space or the accept button. The status bar shows hints for the active tool. A tool can refuse to activate, with a tooltip saying why, when the selection does not suit it.

A commit keeps the result the preview built, so what you saw is what the scene gets. Combine and Refine therefore finish at once instead of computing again, and applying Refine while its preview is still computing waits for that preview. On a large mesh, Combine shows a progress bar naming the step it is on, and closing Combine stops it at once.

When a tool cannot do its job, an alert names the mesh, with a link, and gives the reason. Refine and Split leave the mesh as it was, for example on a mesh that is not a closed solid or a cut that cannot be computed. Bake skips a decal whose SVG it cannot read, and fails on a texture map whose image holds no pixels. Refine's panel and a decal's or texture map's card also show the reason.

## Two levels of mesh editing

Tools that change a mesh work at one of two levels, and the level explains their behavior.

| Level | What the tool assumes | Consequence for you |
|---|---|---|
| Polygon | It edits triangles and faces directly, then runs a fast repair | Works on imperfect scans; asks to Bake modifiers and transforms first |
| Solid | The surface is closed and stays closed | Booleans and similar operations need a closed mesh to give a correct result |

Tools that edit triangles cannot see through a modifier stack, so they ask to [Bake](Workflow-Bake-an-SDF-to-a-Mesh) first. Tools that change a mesh's triangles are also unavailable on a skinned mesh, because its per-vertex weights would no longer match.

## Modifiers and wizards

**Modifiers** are the core modeling substrate: every non-destructive edit is one. Building a result from modifiers by hand gives full control, but takes many steps.

A **wizard** is a tool that builds that result for you. It places nodes and modifiers that you could also have created by hand in the inspector, and it hides the bookkeeping. After a wizard finishes, its output is ordinary nodes and modifiers, which you can adjust in the inspector like any other.

| Wizard | What it sets up |
|---|---|
| Combine | Boolean modifiers between selected bodies |
| Jig | Pins, wires and constraint modifiers for alignment and simple mechanisms |
| Hair | Hair modifiers on regions of a mesh, plus the fields that bend the strands and the walls they end on. A wall is either a shape the tool fits, or an existing mesh or some of its face groups |
| Skeletonize | A lattice modifier on a body, limited to other selected bodies |
| Array | An Array modifier, with the pins or wire that locate its layout |

Producing modifiers alone does not make a tool a wizard. Clay, Split and Text also create modifiers or nodes, but each does one direct job.

## Tool families

| Family | Tools | Purpose |
|---|---|---|
| Arrange | Move, Part | Place objects, and separate or group them |
| Build | Clay, Combine, Text | Create clay bodies, combine bodies with booleans, create text as SVG for decals |
| Cut and fix | Split, Repair, Refine | Cut with a plane, find and fix mesh defects, simplify or refine triangles |
| Regions | Scribe, Paint, Trace, Crack | Divide a surface into face groups, by shape, by brush, by drawn line, or by crack line |
| Construct | Pin, Wire, Jig | Place reference points, connect them, and add constraints between them |
| Generate | Hair, Skeletonize, Array | Printable hair, strut lattices, and repeated copies |
| Character | Rig, Pose | Build a skeleton and weights, then pose and play clips |
| Finish | Bake | Turn modifiers, clay, decals, poses and textures into plain geometry |
| Present | Demo | Play or edit a guided tour of the scene |

The Character tools build and drive a [rig](Architecture-Rigging-and-Pose): a skeleton, per-vertex weights, poses and clips.

### Regions: face groups

Several tools share one idea: a **face group**, a set of triangles that belong together. Face groups drive coloring for multi-material prints, which faces Crack separates, and which faces Extrude pushes. Kelcite computes initial groups in the background after import. Scribe finds clean, separable regions from the surface's shape automatically. Paint works on triangles directly, for quick marking. Trace draws lines on the surface for clean edges that Paint and [Split](Workflow-Split-and-Cut-a-Model) can follow. All of them read which faces meet the [same way](Architecture-Geometry-Engine), so a brush, a fill or a trace crosses the seams of a textured mesh and the separate triangles of an STL as it crosses any other edge.

### Construct: pins, wires and constraints

Pins and wires carry no meaning of their own; tools give them one. A pin can be the plane of a clay split, the direction of an extrude, the origin of an Array or a lattice pattern, a stop on a demo tour, or the place where Text puts its label. A wire connects two pins. A **constraint** modifier states a relationship (a position, an orientation, a distance, a hinge or ball joint, an alignment), and a solver moves the constrained nodes until the relationships hold. Constraint values can refer to named parameters in the setup table, and a length typed there reads in the [scene unit](Architecture-Units-and-Files).
