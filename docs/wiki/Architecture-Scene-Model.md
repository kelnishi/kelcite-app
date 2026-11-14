# Architecture: Scene Model

A Kelcite document has to stay editable after every change, so it records most edits as instructions on a node rather than as changed triangles. Its contents are a tree of nodes, the geometry they refer to, and the modifiers stacked on them.

## Nodes

A document is a tree of **nodes**. The tree is what the Object Browser shows. In a large scene, Ctrl+F opens a search field that filters the tree by name as you type. A node has a name, a transform (position, rotation and scale relative to its parent), a visibility flag and, depending on its kind, some data of its own. Moving a parent moves its children with it.

| Kind | What it is |
|---|---|
| Group | A container with no geometry of its own |
| Mesh | Triangles, with materials, face groups, and optional colors and texture coordinates |
| Clay | A body defined by a shape function rather than triangles |
| Pin | A point with an orientation, placed on or near a surface |
| Wire | A connection between pins |
| Trace | A line drawn along a mesh's surface |
| SVG | A vector drawing, used for decals and text |
| Image | A picture, used for decals, displacement and BitCanvas painting |
| Hair field / wall | A field bends the direction hair grows in; a wall is a shape the strands end on |
| Bone | One joint of a rig |
| Demo | An ordered tour of the scene |

Any node can carry notes, written in Markdown, which can also show as a flag above the node in the 3D view.

## Shared geometry

A mesh node refers to its geometry rather than holding it. The document keeps geometry, images and clay volumes in their own collections, and nodes point into them. Undo can therefore keep many steps without duplicating unchanged meshes, and autosave writes only what changed.

## Modifiers: the non-destructive stack

A **modifier** is an edit stored on a node rather than applied to its geometry. The node keeps its original shape, and the modifier is evaluated on top of it every time the result is needed. You can disable, reorder, adjust or delete a modifier at any point, and the original comes back.

Modifiers run in the order the Modifiers panel lists them. Each one takes the result of the one before.

![A scene tree beside the modifier stack of one mesh: its geometry, then a Difference with another node, then a Decal from an SVG node, then the result](images/arch-scene-model.svg)

| Modifier | Applies to | What it does |
|---|---|---|
| Union, Difference, Intersect | Mesh, clay | Booleans against other nodes, or against every node on chosen layers. Where a tool shapes the body, its faces take the tool's material |
| Round, Threshold | Clay | Grow, shrink or round off a clay body |
| Split Plane | Clay | Cut away one side of a plane defined by a pin |
| Constraint | Nodes | Keep a position, orientation, distance or joint relationship |
| Decal | Mesh, clay | Project an SVG, image or BitCanvas onto the surface |
| Texture Map | Mesh, clay | Use an image to displace the surface, or, on clay, to color it |
| Mask | Image | Clip an image to a shape before other modifiers read it |
| Extrude | Mesh | Push chosen face groups outward or inward, with draft, twist or a path |
| Loft | Trace, SVG, text, closed wire, pin | Build a solid through a series of sections, as a part of its own |
| Hair | Mesh | Grow printable hair from one material's faces |
| Lattice | Mesh, clay | Turn the body into a strut lattice |
| Array | Mesh, clay, rig | Repeat the body, or a rig's posed figure, in a line, a ring or a grid |

A boolean's other bodies stay in the scene as ordinary nodes. Moving one of them updates the result. A boolean on a mesh needs a closed solid, and on one that is not, the view leaves the boolean out with a message saying why, as Bake would.

### Array

An **Array** draws copies of its node without adding nodes to the scene. A pin, a wire, another node or one of the scene's construction planes locates the layout, and the layout can also stand on a mesh's surface. Each copy can differ from the last by **variations** — a turn, a scale, a random offset, or, for a [rig](Architecture-Rigging-and-Pose), a different frame of an animation clip. Changing the original changes every copy. Baking an Array makes each copy a node of its own. Clay copies are separate bodies, so where two touch they meet with a crease instead of a blend.

An Array stands in the scene's own frame, where up is Y, whatever Up Axis Preferences sets for [files](Architecture-Units-and-Files). An Array made in a document while Up Axis was Z can open with its copies tipped a quarter turn onto their side; Flip and Turn under Orientation stand them back.

## Bake: when the stack becomes geometry

Most of the outside world understands only triangles. A slicer cannot read a [clay body](Architecture-Geometry-Engine) or a pending boolean. **Bake** evaluates a node's modifiers and writes the result into plain mesh geometry, after which the modifiers are gone. It takes time in proportion to the detail you ask for, and [baking clay to a mesh](Workflow-Bake-an-SDF-to-a-Mesh) walks through the settings.

Some tools need baked geometry to work. Tools that edit triangles directly (Part, Repair, Refine, and Split on meshes) ask to bake first when the selection holds modifiers or clay. A rotation or scale alone is baked into the vertices without asking. Export to STL, 3MF or OBJ reports unbaked bodies instead of writing a shape the file cannot represent. Kelcite's own GLB format keeps the modifiers, so a saved document stays editable.

## Layers

A **layer** is a named collection of nodes with its own visibility switch. A node belongs to at most one layer. Layers show and hide sets of nodes together, and they also serve as a boolean's tool list. A boolean that targets a layer uses every mesh on that layer, including ones added later.

## Setup table

The setup table holds named parameters and measured features, such as a wire's length or the distance between two nodes. [Constraint](Architecture-Tools-and-Modifiers) values can be expressions that refer to these names. Changing one parameter then updates every constraint that uses it.

## Undo

Every edit writes an undo step: a node move, a tool's commit, and the smaller edits below.

| Edit | Undo step |
|---|---|
| A change in the Info panel (colors, SVG, text and image settings, primitive settings, wire tangents) | One per change; a slider drag undoes as a whole |
| Adding, renaming, hiding or removing a layer | One each |
| Renaming an object in the Object Browser | One |
| Solve in the setup table | One |
| Recompute normals | One step covers every selected mesh, and undo brings the old normals back |
| A color added to Paint's palette with Shift+Plus | One; the palette comes back as it was |

The history is [saved with the document in the browser](Architecture-Storage-and-Privacy), so it survives a reload. Some tools also keep a short history of their own while they are active, which the same undo buttons and keys step through.
