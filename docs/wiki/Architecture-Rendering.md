# Architecture: Rendering

The 3D view is where you judge a model before it prints, so it gets the largest share of the window. Its colors and outlines report state — what is selected, hovered, referenced or weighted.

## The viewport

The 3D view is drawn with Three.js on WebGL. The interface around it is a set of docked panels. Panels can be docked, floated over the view, or collapsed.

Drawing runs on the main thread and the GPU. Geometry work runs elsewhere, in the [geometry engine](Architecture-Geometry-Engine)'s workers, so a long computation does not stop you orbiting the view. A large mesh's drawn geometry is built in a worker too, and the view picks a large mesh through the graphics card that draws it; [performance](Architecture-Performance) covers both.

## How each kind of body is drawn

| Body | How the view draws it |
|---|---|
| Mesh | Its triangles, with materials, vertex colors and textures. Paint, several materials, Show face groups and flat shading all draw from one copy of the mesh. Decals on a mesh draw as an overlay on its surface |
| Large mesh that is still being built | A soft fog of a neutral color fills its bounds until its geometry arrives |
| Clay | Ray marched: a shader steps along each pixel's view ray until it reaches the clay's surface. No triangles exist until Bake |
| Mesh turned into clay | Ray marched through a distance field built from the mesh |
| Hair, lattice | Previews of the generated bodies, updated as you change the modifier |
| SVG, image | Flat shapes and pictures placed in the scene |

Ray marching is why clay looks perfectly smooth at any zoom: the view reads the shape function directly. It is also why a clay body can look finished in the view and still need [Bake](Workflow-Bake-an-SDF-to-a-Mesh) before a slicer can read it.

![A mesh is rasterized; clay, and a mesh turned into clay through its distance field, are ray marched; both reach the 3D view, and Bake turns clay into a mesh](images/arch-rendering.svg)

## Why objects appear in stages on open

The browser has to compile a shader program for each kind of material before it can draw it. A document with many materials, such as the demo, would freeze the app while that happened. Kelcite instead compiles them in the background where the browser supports it, and leaves each object out of the view until its shaders are ready. The Object Browser shows a spinner beside a [node](Architecture-Scene-Model) that is still preparing. A load screen covers the view until the document is ready to use.

An object already on screen does not wait that way. A clay body whose edit needs a new shader, such as a displaced or lattice body, keeps showing its last picture until the new one compiles. A compile can still stop the view from drawing, so when one runs past about a tenth of a second, the notice "Preparing shaders — the view may pause briefly" shows at the top left of the view until the compiles are done.

## What the colors mean

Color in the view communicates state. Each meaning has its own color family, and colors are paired with shape (dashed outlines, boxes, markers) so color is never the only signal.

### Selection and hover

| What you see | Meaning |
|---|---|
| Dashed box with dimension labels, bright cyan | One object is selected |
| Dashed box with dimension labels, green | A group, or several objects, are selected |
| A box per object, cyan | Objects under a band (drag) selection while you drag |
| A dashed cyan box per object, while a quick edit's Apply is hovered | Where the quick edit would put each object; nothing changes until Apply |
| Orange box | The object you are hovering in a panel, such as an Object Browser row or a node reference in the inspector |
| Yellow glow on part of a surface | The face group under the pointer, in tools that pick face groups |
| Magenta or thicker line | A selected trace or wire, which mark their own selection instead of a box |

The dimension labels on the selection box use the [display unit](Architecture-Units-and-Files) from Preferences.

### Axes and construction

| What you see | Meaning |
|---|---|
| Red, green, blue lines | The X, Y and Z axes |
| Violet diamond | The origin |
| Diamond with a blue outline | The point the view orbits and zooms about, shown while the view moves and fading once it rests |
| Tinted planes and axis lines | Reference axes, planes or the origin that a tool or panel currently points to, shown while you pick or edit that reference |

The view's up is the scene's Y. The Up Axis preference applies only to how [files](Architecture-Units-and-Files) are read and written. Preferences also turns the pivot diamond off, makes a drag orbit about the surface under the cursor instead of the orbit target, and sets whether the pivot moves to the body at the center of the view when a pan ends. A 6DoF controller's slides do not count as a pan, so it never moves the pivot this way.

### Weights

The Rig tool's weight view colors each vertex by how strongly a [bone](Architecture-Rigging-and-Pose) moves it, on a ramp from dark (none) to light (full). The ramp is data: its purples, magentas and oranges mean an amount, never a selection or a warning. Preferences can change the ramp's colors for each theme.

## Display aids

| Aid | What it does |
|---|---|
| Grid and shadows | A ground grid and soft shadows, to read height and contact |
| Wireframe | Draws triangle edges over the surface as continuous lines at a weight Preferences sets. On a large mesh a worker lists each edge once, the wireframe wipes in across the model when the mesh's geometry finishes building, and turning it off gives back its memory |
| Zebra stripes | Reflective stripes that show breaks in surface smoothness |
| Face groups | Colors each face group, to check how a surface is divided |
| Labels | The selection box's dimension labels, measurements and note flags |
| X-ray | Draws a mesh transparent, so geometry inside it is visible |
| Note flags | Shows a node's notes as a flag above it |
| Light and dark themes | Chosen in Preferences; the view's overlays follow the theme |

## Depth range

A view that zooms from a part's detail to a building's scale needs one depth range to hold both. The ground, grid, thin layers, decals and bodies stay solid from 0.1 mm away to a building's scale, so a 0.2 mm layer holds together at 10 m and nothing closer than half a millimeter is cut away.

## Preview intent

A tool's preview that takes longer than 300 ms (a large mesh) draws a violet hint of the result, so the wait reads as work in progress. A preview that finishes sooner shows nothing.

| Tool | The hint |
|---|---|
| Bake | A grid over the clay |
| Refine | Density lines at the new setting |
| Skeletonize | A grid at the lattice's spacing |
| Clay | A grid at the field's cell, on clay made from a mesh |
| Decals | An outline where the decal will land |
| Combine, Move | The tool body's outline through the target |
| Paint | A rim on the ghost where the stroke painted |

Where no tool preview marks the wait, a large mesh being rebuilt shows a neutral gray wave for as long as it is out of date, and its row in the Object Browser shows a spinner. When Enter can cancel the work, the spinner is a button that names the operation and the body.
