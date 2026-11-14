# Surface and Refine Tools

The third and fourth groups in the tool palette work on a mesh's own triangles. They separate shells, close holes, divide the surface into regions, color those regions, and rebuild the triangles themselves. **Face groups** — sets of triangles that each carry one material — tie them together: Crack, Scribe and Paint make face groups, and [printing](Workflow-Prepare-for-Printing), [Hair](Workflow-Hair-and-Strands) and the face-group [modifiers](Tool-Modifiers) read them. **Alt+F** shows face group coloring in the view.

| Tool | Key | What it does |
|---|---|---|
| [Part](#part) | **O** | Separates a mesh's parts, or merges meshes |
| [Crack](#crack) | **K** | Divides a surface along crack lines and lifts out capped pieces |
| [Repair](#repair) | **R** | Makes a mesh manifold |
| [Scribe](#scribe) | **G** | Segments a surface into regions from its shape |
| [Paint](#paint) | **P** | Paints face groups, or the pixels of a BitCanvas |
| [Refine](#refine) | **T** | Simplifies, subdivides or hulls a mesh |

## Part

Part separates or joins meshes at the polygon level, without a boolean. On one mesh, it splits the mesh into separate meshes, in a group. Each disconnected shell becomes a part, and so does each piece that touches the rest only at a corner, along an edge or across a face; a fin standing off a solid becomes a part of its own. On several, it merges them into one mesh, and overlapping volumes stay intersecting.

## Crack

Crack divides a surface into face groups along crack lines, and extracts chosen groups as capped pieces.

- Click a face to grow a crack line. **Crease angle** sets how sharp a fold must be to divide the surface.
- **Bite radius** sets how far a crack keeps its heading before it turns, so a larger radius makes straighter cracks. Hold **Shift** to paint a guide that steers the crack.
- Select a crack to see its control points. Drag a point to move it, click the crack to add one, and press **Delete** to remove the selected one.
- On a mesh that already has face groups, every boundary between two groups shows as a crack. With **Keep existing groups** on, those boundaries stay group boundaries at any angle.
- Press **K** to switch to **Part** mode for extraction, then click the groups to separate.
- **Cap holes** closes each cut, and **Curved cap** makes the cap follow the surface's slope, curving the way the surface around the hole faces. Two regions that touch at a single corner get a cap each.

A few triangles wound inside out do not split off the faces around them, so the mesh groups as its clean copy does. A face stored twice, or a fin standing off a solid, is a group of its own, and two parts touching along a face keep separate groups. When the analysis fails, the panel shows the error.

## Repair

Cutting, painting and printing all need a closed surface. Repair patches holes, welds open cracks, removes fins and duplicate faces, and reorients flipped faces — the fixes a [scan needs before printing](Workflow-Repair-a-Scan-for-Printing). It opens only on a mesh that is not manifold.

- Holes are outlined in red; click one to patch it. A patch is wound to match the triangles around it.
- **R** cycles the patch strategy: **Liepa**, **RBF**, **Star** or **Smooth**.
- The **Fins and Duplicate Faces** row lists the faces left over where three or more faces share an edge. **Remove fins and duplicate faces** removes them when you apply. A fin's open edges are not listed as a hole.
- **Flipped Normals** are found part by part. The inner wall of a hollow part, which faces into its cavity, is not reported; a sheet, or a part open across a quarter of its surface or more, is left as it is wound. **Fix Normals** turns each separate part outward on its own.
- **Tab** patches every hole, fixes normals and removes fins (**Repair All**).

Repair keeps the face groups, materials, texture coordinates and vertex colors of the faces it keeps. A patch that gets a face group of its own (**Face groups for patches**) takes the next group after the mesh's.

The "not manifold" notice counts open edges, edges shared by three or more faces, and pinched corners, and its numbers match the Repair panel's. For a mesh with fins, duplicate faces or faces wound against their neighbors, it offers **Repair now**, which removes the extra faces and winds every face outward in one step that Undo reverses. Importing a file never changes a mesh on its own.

## Scribe

Scribe segments a surface into clean, separable regions from its shape, without painting.

- After the shape analysis, seeds set the regions. **G** adds a seed automatically, and **Tab** places one by click.
- Crease lines show on an STL import as on other meshes, and the boundary lines follow the mesh through an edit that keeps its face count.
- **Shift+scroll** over a region adjusts its bias.
- `\` switches the output between face groups and boundary traces.
- Apply with **Paint**, or **Space**.

## Paint

Paint assigns face groups triangle by triangle, for marking surface features and keeping their edges.

- **Fill** floods a region bounded by traces and face-group borders. **Brush** paints under a sphere, sized with **Shift+scroll**. **P** switches between them.
- **Tab** cycles the palette, and **Shift** with the **+** key adds a color, which Undo removes again.
- The brush and Fill spread across the triangles of an STL and across seams where a mesh's vertices are split.
- On a BitCanvas, Paint paints pixels: click paints, Shift-click erases and Alt-click picks a color. With **Unproject** on, it paints the canvas where it lies on a body.

On a mesh with a Difference, Intersect or Union [modifier](Tool-Modifiers), Paint shows the modifier's result as a ghost. The faces the tools cut or add show in the tools' materials, and the faces the cut leaves of the mesh take the color as you paint.

A ghost that updates slowly stops updating during strokes. When its first update in a stroke is slower than the **FPS threshold**, it hides for each stroke and updates once when you let go, until Paint closes. Both **Suspend slow updates during paint** and **FPS threshold** are in [Preferences](Workflow-Keyboard-Shortcuts-and-Preferences), under General.

Smooth region borders come from a [trace](Tool-Construction-and-Rigging) drawn first, which Fill then stops at. Paint is one way of [coloring a model](Workflow-Paint-Decals-and-Texture-Maps), alongside decals and texture maps.

## Refine

Refine rebuilds a mesh's triangles at the manifold level. The panel shows the triangle count before and after, and the preview of the last slider value stays on screen while the next one computes. Applying writes exactly the result the preview shows.

| Operation | What it does |
|---|---|
| **Simplify** | Reduces the triangle count within a **Tolerance**, the largest surface deviation allowed |
| **Refine** | Subdivides, keeping edges sharper than **Sharp angle** |
| **Hull** | Replaces the mesh with its convex hull |

Simplify and Hull remove texture coordinates, and Hull also removes per-face materials.

When a mesh cannot be simplified, refined or hulled — because it is not a closed solid, memory ran out, or the background worker stopped — the panel shows the reason under the options. Applying then leaves the mesh as it was, with an alert that links to it and gives the reason; a result with no triangles is reported the same way.

The demo's **Refine** stop shows a ball going from 12,096 to 1,284 triangles.
