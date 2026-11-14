# Create and Bake Tools

The fifth group in the tool palette makes new geometry, and almost all of it starts live. Clay, text decals, hair, lattices and arrays are computed from their settings, so you can keep adjusting them until the shape is right. Most of these tools are wizards that set up a [modifier](Tool-Modifiers), which stays editable afterward. Bake, at the end of the group, turns live geometry into ordinary triangles for printing and export.

| Tool | Key | What it does |
|---|---|---|
| [Clay](#clay) | **A** | Creates an editable clay (SDF) primitive |
| [Text](#text) | **X** | Labels parts with text |
| [Hair](#hair) | **Y** | Grows printable hair from a color region |
| [Skeletonize](#skeletonize) | **L** | Turns a body into a strut lattice |
| [Array](#array) | **D** | Repeats a mesh, clay or posed figure in a line, ring or grid |
| [Bake](#bake) | **B** | Converts live geometry into meshes |

The palette's last tool, [Demo](#demo), plays or edits a scene's tour.

## Clay

Clay creates a [signed-distance](Architecture-Geometry-Engine) primitive at the pointer: a **Sphere**, a **Platonic** solid, a **Cylinder**, a **Polygon** or a **Profile**. The shape is a distance field rather than triangles, so it stays editable until baked; [sculpting with clay](Workflow-Sculpt-with-Clay) works on that editable shape.

- **A** cycles the primitive type.
- **Round**, **Tension** and **Draft** shape it.
- The gizmo places it. Hold **Shift** to snap to surfaces and **Alt** to snap to pins.
- Apply with **Create**, or **Space**.

## Text

Text labels parts. With a pin on a mesh selected, the text becomes a Decal modifier on that surface and the pin is used up. Otherwise it is a text outline on the ground.

The panel sets the font, size, alignment and spacing. Markdown sets bold and italic. Apply with **Create**.

## Hair

Hair is a wizard for [3D-printable hair](Workflow-Hair-and-Strands). A click on a color region adds a Hair modifier whose strands grow from the region out to a wall.

- The panel shapes the strands, and a click on a strand excludes it.
- **Y** cycles the mesh's Hair modifiers, and **M** cycles the surface preview.
- **Placement** chooses the strand layout: **Grid**, **Poisson**, **Stratified** or **Cellular**.

## Skeletonize

A lattice makes a part lighter while keeping its outline. Skeletonize turns the first selected body into a strut lattice, live as a Lattice modifier until baked.

- **Pattern** chooses the lattice: **Truss** or **Voronoi** cells linked over the body, or a repeating unit cell cut by it (cubic, body-centered cubic, face-centered cubic, octet truss, Kelvin foam or diamond).
- **Deep** fills the volume, **Shell** follows the surface, and **Shell + volume** does both.
- Other selected bodies limit the lattice to their region.
- A Pattern pin sets where the pattern starts and which way it runs. Moving or turning the pin moves the pattern through the body.

Baking replaces the body with the lattice. The baked lattice reports any loose pieces, which are struts cut off from the rest.

## Array

Array repeats a mesh, clay or a rig's posed figure in a line, a ring or a grid, live as an Array modifier until baked. Setting one up runs in order, from the body to the layout to the variation:

1. Select the body to repeat and press **D**.
2. Pick a layout, such as **Line**, **Ring**, **Grid** or **Spiral**. The tool makes the pins the layout stands on.
3. Drag the handles in the view to set the spacing, or a ring's radius and arc. **]** and **[** add and remove copies.
4. Choose the **First Variation**, which changes each copy: a frame of the rig's clip, a rotation, a scale or a random jitter.
5. Click **Done**. The Array modifier's card in the Modifiers section edits the array from then on.

With two bodies selected, the first is repeated and the second is the surface it lies on. With the array selected, clicking a copy switches it off, for a gap such as a gate in a row of fence posts. **Materialize Nodes** makes each copy a node of its own.

An array stands in the scene's own frame, where up is Y, whatever **Up Axis** is set in [Preferences](Workflow-Keyboard-Shortcuts-and-Preferences); that setting only says how files are read and written. An array made in an older document while Up Axis was Z can open with its copies tipped a quarter turn onto their side. **Flip** and **Turn** under **Orientation**, on the Array card, stand them back up.

The demo's **Array** stop shows dice whose pips are arrays on the cube's faces.

## Bake

Live geometry has no triangles of its own, so STL, 3MF and OBJ files cannot hold it until Bake writes it out. Bake converts live modifiers, clay, decals, texture maps, hair, lattices, arrays and poses into mesh geometry. Each operation has a card with its own settings and preview; clay adds a [meshing method and a cell size](Workflow-Bake-an-SDF-to-a-Mesh). Apply with **Bake**, or **Space**.

- A card whose preview cannot be made, such as a decal or texture map, shows the reason.
- When a GPU meshing method has to mesh clay on the CPU, the status line says why.
- An operation that cannot be baked is left as it is, with an alert that names its node and gives the reason.

## Demo

Demo plays or edits a scene's tour. Each stop frames a pin and selects nodes, and its title and caption are the pin's name and notes.

- **Play** steps between stops.
- **Edit** records the current view as a stop, and sets what each stop selects.

The tour that ships with Kelcite is not a file in the file browser. It starts over each time it opens, and the app keeps a copy on your device for offline use, replaced when the app updates. A link to it (kelcite.app/glhf/kelcite-demo.glb) opens it on any browser. The first edit to the tour says that its changes are not kept and offers **Save a copy**, which opens as a file of your own, named kelcite-demo-copy, then kelcite-demo-copy#1 and so on. The tour's name at the top of the window saves a copy too. Playing the tour turns **Labels** on if they are off, so its flags show, and they stay on when the tour ends.

A scene with a demo also plays it from an overlay while no tool is open. The overlay's hide button clears it from the view, and **Show demo**, in the demo node's Info panel, brings it back. The [demo scene](Workflow-Getting-Started) is such a tour, with a stop for every tool.
