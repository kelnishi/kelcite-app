# Architecture Overview

Kelcite is a 3D modeling app for 3D printing, and it runs entirely in your browser.

## Why client-side

Kelcite edits files that people often do not want to upload: unreleased products, scans of their own bodies, client work. Keeping every step on your machine removes the question of where a model went. It also means the app keeps working with the network off once it has loaded, so a laptop in a workshop without Wi-Fi is a supported setup.

The cost is that your computer does all the work. A boolean on a dense scan or a high-resolution Bake takes as long as your CPU and GPU need, with no server to hand it to. Much of the architecture exists to keep the interface responsive while that work runs, and [performance](Architecture-Performance) covers how that holds for meshes of millions of triangles.

## How it is delivered

Kelcite is a progressive web app (PWA). The first visit downloads the app and stores it in the browser's cache; later visits start from that cache. A newer version installs in the background and takes effect on the next load, or immediately when you accept the update prompt (the open document is saved first).

There is no account, no login and no server-side copy of your work, because [documents stay in the browser](Architecture-Storage-and-Privacy) until you save a file.

## Layers

The app has four layers. Each one hands work to the next one down and gets results back.

| Layer | What it holds | Where it runs |
|---|---|---|
| Interface | Docked panels, the tool palette, the inspector, dialogs | Main thread (React) |
| Scene state | The document: nodes, modifiers, geometry, materials, layers, undo history | Main thread (in-memory store) |
| Viewport | The 3D view, gizmos, selection and hover overlays | Main thread and GPU (Three.js on WebGL); a large mesh's drawn geometry is built in a worker and picked on the GPU |
| Geometry engine | Booleans, meshing clay, repair, Refine, mesh analysis, segmentation, rigging solves, file parsing, and Paint's brush preparation and modifier preview on a large mesh | Web workers, mostly WebAssembly, and the GPU (WebGPU) where available |

The interface and the [viewport](Architecture-Rendering) read the scene state; tools change it. Anything slow goes to a worker in the [geometry engine](Architecture-Geometry-Engine), and its result comes back as a scene edit.

![The interface, viewport, scene state, geometry engine and browser storage inside the browser; only the app comes in, and a document leaves only as a saved file](images/arch-overview.svg)

## The editing model

A document is a [tree of nodes](Architecture-Scene-Model). A mesh, a clay body, a pin, a wire, an image and a bone are all nodes. Many changes are stored as **modifiers** on a node rather than written into its geometry, so you can adjust or remove them later. **Bake** turns a node and its modifiers into plain mesh geometry, which is what printers and most other software need ([baking clay to a mesh](Workflow-Bake-an-SDF-to-a-Mesh) walks through it).

[Tools](Architecture-Tools-and-Modifiers) are the way you change the scene. Most show a preview while you adjust them and apply the change when you commit (Space, or the accept button). Every committed change is an undo step.

## Open source components

Kelcite builds on open source libraries for the heavy lifting. These are the ones a user is most likely to meet by name.

| Library | Used for |
|---|---|
| Three.js | Drawing the 3D view |
| Manifold | Booleans and solid-mesh operations |
| Assimp | Importing the long tail of mesh formats |
| Open CASCADE (via an optional download) | Importing STEP files |
| ONNX Runtime with SlimSAM (via an optional download) | The Rig tool's Refine with vision |
