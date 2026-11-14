# Cut and Join Tools

The second group in the tool palette changes how many solids a model is. Split divides one model with a plane, and Combine joins, subtracts or intersects several. Both work at the [manifold level](Architecture-Geometry-Engine), so both produce closed solids that print, and both need a [manifold mesh](Workflow-Repair-a-Scan-for-Printing) to start from. [Crack and Part](Tool-Surface-and-Refine), in the next group, divide and separate meshes without a plane or a boolean. [Cutting a model into printable pieces](Workflow-Split-and-Cut-a-Model) can call on all four.

| Tool | Key | What it does |
|---|---|---|
| [Split](#split) | **S** | Cuts a model with a plane, or along a closed Trace, into A and B pieces |
| [Combine](#combine) | **C** | Boolean union, intersection and difference |

## Split

Split cuts along a plane and puts a [pin](Tool-Construction-and-Rigging) on each cut face, which marks where the pieces meet.

- Drag a line across the view to set the plane.
- **X**, **Y** and **Z** align the plane to an axis.
- **Shift+scroll**, or **↑** and **↓**, offsets it; with Shift, the arrow keys move it ten times as far. **R** resets the offset.
- `\` flips the plane. **S** switches the gizmo between translate and rotate, and **Tab** cycles its pivot.
- Click a cut loop in the scene to leave that loop uncut.
- **Add centroid pin to result bodies** adds a pin at the center of each piece.
- **Cap strategy** chooses how each cut face is closed: **Flat**, **Liepa**, **Star**, **RBF** or **Smooth**. **Clearance** sets how close to the plane a surface point may lie before it counts as on the plane.
- **Trace**, at the top of the panel, cuts along a closed Trace instead of a plane, with a curved cut face where the Trace is not flat.

On a mesh, Split makes an **A** piece (above the plane) and a **B** piece (below). On clay, it adds a **Split Plane** [modifier](Tool-Modifiers), which keeps the side the pin's up axis points to and stays editable.

A mesh with a few triangles wound inside out still splits, and its pieces come out wound outward. When the cut cannot be computed, or the pieces on either side of a loop you clicked off cannot be joined again, the mesh stays as it was and an alert links to it with the reason.

## Combine

Combine is a wizard for booleans. The first selected node is the target, and the nodes added with Ctrl-click (Cmd-click on a Mac) are the tools.

| Operation | Result |
|---|---|
| **Union** | Joins the tool shapes to the target |
| **Difference** | Cuts the tool shapes out of the target |
| **Intersect** | Keeps only the volume the target shares with the tools |

- **C** cycles the operation.
- Without **Bake on apply**, the tools move under the target and drive a live Boolean modifier, so you can still move them. With it, the result becomes one mesh.
- **Consume tool meshes** deletes the tools after combining, and **Reparent tool meshes** moves them under the target as children.
- A boolean of large meshes builds in the background, and a progress bar shows its step. Closing Combine stops it, and **Apply** keeps the result the preview built.

The faces a tool cuts or adds take the tool's material on a mesh, and the tool's color on [clay](Workflow-Sculpt-with-Clay). A live Boolean shows the result [Bake](Workflow-Bake-an-SDF-to-a-Mesh) will write, face for face. On a mesh that is not a closed solid, the Boolean is left out of the view, and an alert says why.

A Boolean modifier's **Smooth** control rounds the seam into a blend over the distance set.
