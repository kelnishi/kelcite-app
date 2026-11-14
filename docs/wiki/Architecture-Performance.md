# Architecture: Performance

A scan or a fine Bake reaches hundreds of thousands to millions of triangles, and Kelcite edits it inside one browser tab. The tab has one main thread, which draws the interface, handles input and runs the 3D view. Any pass over every triangle on that thread is a pause you see. A large mesh stays responsive because such passes go elsewhere — to workers, to the graphics card, or into slices of a few milliseconds between frames.

The [geometry engine](Architecture-Geometry-Engine) says where operations such as booleans run. Drawing, picking, previews and memory follow the same rule. Each timing below comes with the triangle count it was measured at, and another machine gives other times. Where a figure is set against a second one, the second is what the same work cost before the design described with it.

## Work that leaves the main thread

A worker is a background thread with its own memory. Handing it a mesh costs a copy, so a small job runs where it is asked for and a large one goes to a worker, where the round trip pays for itself. A worker keeps a mesh it has been sent, so the next request about that mesh, such as the next value of a slider, sends only its settings.

| Work on a large mesh | What the background task spares |
|---|---|
| Combine's boolean | Three spheres of 1 million triangles each froze the window for about 20 seconds when the boolean ran on the main thread. A progress bar names the step, and closing Combine stops it at once |
| A boolean modifier's result, as the view and Paint show it | The window freezing while a boolean's tool moves, or after each Paint stroke. The previous cut stays drawn until the new one is ready |
| The shading after an edit of the mesh's shape, such as a Repair or a transform bake | A held page of up to 1.5 seconds at 5 million triangles, which the worker brings under a tenth of a second. The mesh keeps its last surface meanwhile |
| Preparing the mesh for Paint's brush | A held page on the first press. If preparing fails, an alert says why and the next press tries again |
| The structures picking resolves against | The view freezing when the pointer first crosses a just-edited mesh in Pin, Trace, Crack, Jig or Scribe |
| A wireframe | The view freezing when the wireframe is switched on. The worker lists each edge once |

A Paint stroke on a very large mesh is stored and drawn a few milliseconds at a time, so letting go of the brush does not stall the view.

## A stand-in while a large mesh first appears

A mesh of 200,000 triangles or more that appears in the scene — after a Bake, an undo or an open — has no surface to show until its drawn geometry is built. That build runs in a worker, and a soft fog fills the space the mesh will take until it lands. A fog reads as something on its way; a solid stand-in would read as the model itself, in the wrong shape. Its color is a neutral that means no selection, construction or warning, and it holds still for a user whose system asks for reduced motion.

At 5 million triangles, building on the main thread held the page about half a second. With the fog, the hold is a few hundredths of a second.

![Two lanes over time: on the main thread a large mesh appears, a fog stands in while the worker builds its drawn mesh, the mesh is drawn, and picks resolve once the worker has built a face list; a click made before then waits](images/arch-performance.svg)

Focusing on a mesh that is still appearing frames it once it has appeared, on its own bounds. A wireframe that is already on wipes in across the model, corner to corner over a quarter second, when the geometry lands. In the second or so after a large mesh appears or changes, a click, a hover or the camera's re-pivot on it waits until the mesh can be picked, then takes effect where it was made.

## One copy of the mesh for every look

A mesh can be drawn several ways: painted into face groups, with several materials, with Show face groups on, flat shaded, or with the face group under the pointer lit. A separate copy for each look multiplied the graphics memory a large mesh took, and switching looks meant building a mesh. Kelcite draws every look from one copy, which records each vertex's material and face group; a look changes the color each class gets, not the geometry. Show face groups, flat shading and hovering a face group therefore show at once.

A boolean's or Extrude's result is drawn the same way, from one copy of the result. Turning on flat shading for a mesh without face groups, or for a modifier stack's result, takes about 0.1 seconds at 5 million triangles, where it took 0.6, and uses a quarter of the graphics memory it used.

## Edits that touch part of a mesh

Most edits change a few vertices or faces. Rebuilding and uploading the whole mesh for each one is what made strokes and drags lag on a large mesh, so an edit updates only what it changed.

| Edit | What is redrawn |
|---|---|
| An edit that only moves vertices, such as a Repair or applying a transform | The vertex positions, without rebuilding the mesh |
| A Paint stroke, and its undo and redo | The faces the stroke touched. The stroke shows as soon as you let go |
| A drag of Extrude's Depth, while the extrusion touches nothing else | The extruded faces, every frame. Letting go builds the final mesh once. At 1.2 million triangles the drag holds about 450 MB less, and the view draws more of its steps |

## Picking through the graphics card

Picking means finding the triangle a ray from the camera meets, for a click, a hover, the Paint brush or the orbit pivot. Testing every triangle is far too slow on a large mesh. A search structure built for each mesh took a second or more to build after the mesh was drawn, held memory of its own, and had to be refit after each pose. The graphics card already holds the mesh, so Kelcite draws it once more into a tiny target around the ray, and the card reports which triangle lies there. A posed character is picked where it stands, with no pause after a pose.

| Action, on meshes of a million triangles or more | Cost |
|---|---|
| A click on a 5-million-triangle mesh | Under 0.1 seconds, where it took about 0.7 |
| A pan's release, an orbit about the point under the cursor, Pin's hover | One frame, where each took about a quarter second |
| The first right-click on a 5-million-triangle mesh | About 0.4 seconds, where it took 1.3 |
| The face-group menu at 1.2 million triangles | About 0.14 seconds, where it took 0.39 |

Smaller meshes, and tool previews such as Bake's preview, Split's halves, Refine's result, Part's pieces, Scribe's segments and clay drawn as a mesh, are picked through a triangle index, which a worker builds for all but the smallest.

## Previews that give way to the next setting

Dragging a slider asks for a new preview at every step. If each preview ran to the end, the one for the value you stop on would wait behind every value you passed. A preview for a value the slider has left stops at its next step, and the last result stays on screen until the next one is ready.

| Preview | Behavior |
|---|---|
| Refine | The preview of the value you let go at shows 0.5 seconds after it on a 400,000-triangle mesh, where finishing the passed values first took 1.3 seconds |
| Bake, cell size | The earlier position's preview stops at the mesher's next step, and the GPU mesher stays loaded for the next one |
| Bake, clay made from a mesh | Its meshing stays loaded while Bake is open; on a 100,000-triangle mesh, a setting change redraws about a quarter faster |
| Skeletonize on clay | A lattice for a setting already passed stops building |
| A boolean on a mesh, while its tool moves | The previous cut stays drawn |
| Clay with a displacement map, while a cutting body is dragged | The last shape stays drawn |

Committing keeps the result the preview built. Combine therefore finishes at once, and Refine writes exactly what its preview shows; applying Refine while the preview is still computing waits for it.

## Memory goes back when a look or tool closes

A browser tab gets a limited share of the machine's memory, and on a large mesh each look, tool and cache holds sizable arrays. Kelcite gives that memory back when the look or tool closes.

| What closes or changes | What goes back |
|---|---|
| Show face groups turned off | About 0.6 GB on a 5-million-triangle mesh |
| Flat shading or the wireframe turned off | The memory that look used |
| Every look tried and turned off again | The page holds about 1 GB less than when looks kept their memory after being turned off |
| Paint closed | The memory the brush used |
| A large mesh edited again and again | Each earlier version's display data; Scribe's cached segmentation when the mesh changes |
| A worker idle for 60 seconds whose WASM heap grew past 512 MiB | The worker ends, which returns its heap. The next job starts a fresh one |

WebAssembly memory grows and never shrinks, so ending an idle worker is the only way a heap that grew for one large job returns to the browser.

Some tools also start smaller. Opening Paint on a 5-million-triangle mesh takes about 110 MB, where it took 1.1 GB, and the brush is ready in about a third of the time. The first Bake preview of clay on a GPU method takes about 410 MB at 1.2 million triangles, where it took 1.3 GB. A repaired mesh takes about a third of the memory, and saves smaller, because its triangles share their corners.

## Shader compiles and the "Preparing shaders" notice

The graphics card draws each kind of material with a compiled program, and the browser compiles one the first time a material needs it. In WebGL a compile can stop the 3D view from drawing until it finishes, so a new kind of material can pause the view. Kelcite keeps compiles out of the way where it can. Bake compiles its meshing methods and refinement passes before their first use, so choosing marching cubes or surface nets does not freeze the view for the 1.6 seconds their compile took. An object whose programs are not ready is left out of the frame, and overlays such as the wireframe do not wait behind the scene's programs. A clay body already on screen keeps its last picture while an edit's program compiles ([rendering](Architecture-Rendering) covers how objects appear when a document opens).

When a compile has run for about a tenth of a second, the notice "Preparing shaders — the view may pause briefly" shows at the top left of the view. It goes when the compiles are done, so a pause that follows has a stated cause.

## Paint's slow-update suspension

While you paint a mesh with modifiers, Paint draws a ghost of the modified result over it, and each stroke changes that result. On a large mesh, updating the ghost can take longer than a frame, which makes the brush lag behind the pointer. Paint therefore measures the ghost's first update during your first stroke. If it is slower than the **FPS threshold** (20 fps, a 50 ms update, by default), the ghost stops updating while you paint: it hides for each stroke, updates once when you let go, and stays that way until you close Paint. A mesh whose ghost updates quickly keeps updating during strokes.

While the ghost is hidden by holding Shift or by Ghost Opacity at 0, a stroke does not rebuild the modifiers behind it. The ghost updates once when it shows again, and stays hidden until it matches the mesh.

Two settings in Preferences, on the General tab under Tools, control this ([keyboard shortcuts and preferences](Workflow-Keyboard-Shortcuts-and-Preferences) lists the rest, and [painting](Workflow-Paint-Decals-and-Texture-Maps) covers the brush).

| Setting | What it does |
|---|---|
| Suspend slow updates during paint | On by default. Turning it off keeps the ghost updating during strokes, and ends a suspension already in effect |
| FPS threshold | 1 to 60. How slow an update must be to suspend it |

## The performance overlay and the guard

The **performance overlay** shows where a session's time and memory go. It opens from the menu at the top right, or with Alt+P. It graphs the frame rate, the CPU and GPU cost of a frame, the JS memory and the scene's GPU buffers, and lists what the renderer draws and what the caches hold. For each worker it lists the memory pools and the WASM heap's peak. It also states each time a worker was restarted and why: an idle worker whose heap had grown, or a worker ended to stop a request it could not cancel.

The **performance guard** watches the frame rate, the JS memory and the GPU buffers against limits set under Performance guard in Preferences. Near the JS memory or GPU buffer limit, it first frees the memory pools nothing is using, on the page and in the workers, which changes nothing you see. It warns only if the measure is still over its limit. A warning names the nodes with the most triangles as links and offers to show the overlay; for the frame rate and the GPU buffers it also offers to stop drawing the scene. Stopping is a button you press, and the stopped view has a button to draw the scene again.

## Large-mesh work by operation

Each row names an operation and what it costs on a large mesh.

| Operation | Cost |
|---|---|
| Convert to clay, mesh made clay | The mesh's distance field is built on the GPU where WebGPU is available: about 1 s at 1 million triangles (20 s on the CPU) and about 2 s at 5 million (over a minute). Round takes about 0.6 s at a million, and each step of its slider about 0.3 s. Without WebGPU, or when the GPU cannot hold the mesh, the field is built on the CPU and the Info panel says why |
| Trace Split mesh | Changes only the triangles along the trace, so a mesh of millions of triangles splits in about the time it takes to draw the result. It runs in the background with Cancel. Repair handles defects far from the trace |
| Repair | Detecting and applying fixes run in the background with Cancel |
| Split's plane preview | A worker finds the cut loops, so dragging the plane through a mesh of a million triangles stays smooth. The model itself is tinted blue (Above) and orange (Below) instead of drawn again for each side |
| Booleans | A boolean on a 5-million-triangle mesh completes. One too large for the engine's memory stops with a message that says so |
| Clay shaders | A displaced clay with a decal compiles in about 2 s, from 17 s, so a clay drawn for the first time does not freeze the view |
| Import | The undo step is encoded without freezing the view, and STL files open as shared-vertex meshes |
| Asset Library | A saved mesh takes about a quarter of the space |
| Export | STL and 3MF files of 1.6 million triangles build without freezing the page |
| Lattice | A model of a million triangles or more builds without crashing the page, and faster |

When the scene is preparing, **Show the scene now** appears after 8 seconds and shows the scene while the rest finishes. Objects whose shaders are still compiling stay hidden, with a spinner in the Object Browser, and the notification log names what was skipped.

Two points that were the same point should stay the same in every tool. Vertex positions of meshes made or edited by Import, Combine, Bake, Repair and Commit units therefore snap to one fixed grid, 2^-16 of a unit (about 15 nanometers in a millimeter scene). Exports of those meshes can differ from earlier ones in the sixth decimal place. A document with hair saved before this shows a notice on open, because its hair is regenerated on the new weld.
