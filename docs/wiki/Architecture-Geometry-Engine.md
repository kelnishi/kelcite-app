# Architecture: Geometry Engine

The geometry engine does Kelcite's heavy computation — booleans, meshing clay, repair, segmentation and file parsing — away from the interface, so the app stays usable while an operation takes seconds or minutes. It works on two kinds of body, polygon meshes and clay, and Bake is where one becomes the other.

## Why heavy work runs off the main thread

A browser page has one main thread. It draws the interface, handles your clicks and runs the 3D view. Any long computation on that thread freezes all three until it finishes.

Kelcite therefore runs geometry work in **web workers** — separate threads that compute in the background and send results back. Most of that code is compiled to **WebAssembly** (WASM), which runs close to native speed in the browser. While a worker computes, you can still orbit the view, read panels and cancel where a tool offers it.

| Work | Runs in |
|---|---|
| Booleans for Bake, Combine and Split (Manifold) | Worker, WASM |
| A mesh's boolean modifiers, as the view and Paint show them | Worker for large meshes; main thread for small ones, where the round trip to a worker costs more than the boolean |
| Refine's simplify, refine and hull | Worker, WASM |
| Mesh analysis after import (closed or not, volume, area, face groups) | Worker |
| Importing most formats (Assimp, FBX, STEP) | Worker, WASM |
| Segmenting a mesh into regions (Scribe) | Worker, WASM |
| Meshing clay for Bake | GPU (WebGPU) where available, otherwise a worker |
| Building a mesh's distance field, so a mesh can become clay | Worker |
| Hair, lattice, crack and trace computation | Worker |
| Solving a texture map's or decal's wrap | Worker |
| Auto-rig and automatic weights | Worker |
| Compressing and reading archives (3MF, compressed GLB) | Worker |
| A large mesh's drawn geometry, its shading after an edit, and the structures picking uses | Worker |
| Preparing a large mesh for Paint's brush | Worker |

Moving data between threads has a cost. A large mesh has to be handed to the worker and the result handed back. That transfer is one reason an operation on a dense scan takes noticeably longer than the same operation on a simple model. A worker therefore keeps a mesh it has been sent, and a later request about the same mesh, such as the next value of a slider, sends only its settings.

Drawing and picking a large mesh follow the same rule, with the graphics card doing the picking; [performance](Architecture-Performance) covers how a mesh of millions of triangles stays responsive.

## When a worker stops

A worker can stop partway through a job: the browser runs out of memory for it, its WebAssembly code aborts, or Kelcite ends it to stop a boolean that cannot be interrupted, as when you close Combine. The next request starts a fresh worker and sends it what it needs again. A preview therefore resumes on the next edit or stroke, and a tool that cannot finish shows an error instead of waiting.

WebAssembly memory grows and never shrinks. A worker that has sat idle for 60 seconds with a WASM heap past 512 MiB is ended for that reason, which returns its memory, and its next job starts a fresh worker.

## Two kinds of body

Kelcite works with two representations of a solid, and each is good at different things.

| | Polygon mesh | Clay |
|---|---|---|
| Defined by | Triangles | A shape function: for any point, the distance to the surface |
| Typical source | Imported files, baked results | Clay tool primitives, or a mesh turned into clay |
| Strong at | Exact edges, face-level editing, every file format | Smooth blends, rounding, offsets, booleans that always give a valid shape |
| Edited with | Triangle tools: Part, Repair, Refine, Split, Paint, Scribe | Modifiers: booleans with a blend radius, Round, Threshold, displacement |
| Drawn by | Rasterizing its triangles | Ray marching the shape function on the GPU |

**Polygon meshes** are what printers and other software read. [Tools](Architecture-Tools-and-Modifiers) that change them either edit triangles directly, followed by a quick repair, or work at the solid level and assume the surface stays closed. A [mesh with holes](Workflow-Repair-a-Scan-for-Printing) is still a mesh, but solid-level operations such as booleans need a closed one.

**Clay** has no triangles until you Bake it. Its surface is wherever the shape function crosses zero, so [combining, rounding or growing](Workflow-Sculpt-with-Clay) a body is arithmetic on that function. Clay booleans can therefore blend smoothly, and their result is always a valid solid. A mesh can become clay from its context menu. Kelcite then builds a distance field from its triangles, which takes time on a detailed mesh. That field is cached locally so a return visit to the document skips the rebuild. A mesh with holes still converts, but its inside is an estimate near the holes, and the Info panel says so.

## What Bake does

Bake turns whatever a node currently shows into plain triangles — the boundary where the [modifier stack](Architecture-Scene-Model) becomes geometry.

| Source | What Bake does |
|---|---|
| Mesh with modifiers | Evaluates the stack (booleans, extrudes, decals, texture maps) and writes the result as the mesh. The faces a boolean's tool cuts, adds or keeps take the tool's material, added to the mesh's materials |
| Clay | Samples the shape function on a grid and builds a surface through it (contouring), then refines the result |
| Clay made from a mesh, with nothing that changes its shape | Gives the original mesh back, exactly |
| Posed rig | Writes each skinned mesh in its current pose |
| Hair, lattice | Writes the generated bodies as meshes |
| Array | Makes each copy a node of its own |
| Transform | Writes position, rotation and scale into the vertices |

The view draws a mesh's booleans from the same computation Bake runs, so what you see is what Bake writes, face for face. A boolean on a mesh that is not a closed solid is left out of the view with a message saying why, because Bake would leave it out too.

A posed rig bakes through the [Pose step](Architecture-Rigging-and-Pose), which also checks where the figure passes through itself.

Clay baking is where most of the time goes. The cost grows with the resolution you pick: halving the cell size gives roughly four times the surface to compute. Bake sizes the mesh either by cell size or by a target triangle count, and shows a preview before you commit, so you can trade detail against time and file size ([baking clay to a mesh](Workflow-Bake-an-SDF-to-a-Mesh) covers the settings). On a machine with WebGPU, the contouring runs on the graphics card, which is usually much faster than the CPU path.

![On Bake, a clay body goes to contouring on the GPU or in a worker, a mesh's booleans go to Manifold in a worker, and both come back to the main thread as a baked mesh](images/arch-geometry-engine.svg)

## One reading of a mesh's connectivity

Many tools ask which faces of a mesh meet: Repair, Part, Trace, Crack, Pin, Paint's brush and fill, texture map displacement, and the "not manifold" notice. They share one reading of a mesh, so they agree about it.

| Rule | Consequence |
|---|---|
| Corners at the same position, to within a small fixed tolerance, are joined first | An STL, whose triangles each store their own corners, and a mesh split along texture seams read as one connected surface. Paint's fill, displacement and traces cross those seams |
| Where three or more faces share an edge, they are paired around it so each pair closes one solid | A face left over, such as a fin or a face stored twice, reads as open there, and Repair lists it under fins and duplicate faces |
| The reading depends on the shape, not on the order a file stores its faces | A mesh reads the same way every time it loads |
| Parts that touch only at a corner, along an edge or across a face stay separate | Part, automatic rigging and Separate disjoint regions treat them as separate parts |

The "not manifold" notice counts from the same reading, so its numbers match the Repair panel's: an STL with one fin reports 2 open edges, where a count over its separate triangles would report more than 24,000.

## Why results can differ between machines

The engine aims for results that depend only on the document and the settings. GPU meshing is written to give the same output across runs. Some limits are still physical: a machine without WebGPU uses the CPU path, and a browser tab with less memory can fail on a very large bake where another succeeds. When a GPU meshing method meshes clay on the CPU instead, Bake's status line says why: WebGPU is missing, the clay nests deeper than the GPU evaluates, the mesh needs buffers past the device's limit, or the GPU ran out of memory or reported an error. When a solver cannot produce a result, Kelcite reports the failure; it never substitutes a lower-quality answer.
