# Troubleshooting

Kelcite runs entirely in your browser and keeps your files there, so most problems come from the model itself, the browser's memory, or the browser's storage. Messages in Kelcite say what went wrong and, where they can, link to the tool that fixes it. A notification about an object names it as a link, and clicking the link selects and frames the object.

A problem may already be fixed in a newer version. Clicking the Kelcite name at the top left opens **About**, where **Check for updates** says whether you have the latest version and offers the update when a newer one is out. A fix released between two versions carries a fourth number, as in 0.17.3.1, and is offered like any other version.

## Models and tools

Model problems come from the mesh, from a tool still waiting for input, or from the view and panels.

| Symptom or message | Cause | Fix |
|---|---|---|
| **"Mesh is not manifold. Repair it first."** | The mesh has holes, cracks, flipped faces, fins or duplicate faces. | Run Repair (**R**), as when [repairing a scan](Workflow-Repair-a-Scan-for-Printing). |
| A notice that a mesh **is not manifold**, with **Repair now** | The mesh has fins, duplicate faces or faces wound against their neighbors. | **Repair now** removes the extra faces and winds every face outward, in one step you can undo. Holes still need Repair. |
| Repair does not open | The mesh is already manifold, or nothing is selected. | Select a mesh. If it is manifold, it needs no repair. |
| A mesh's Difference, Union or Intersect shows no cut, with a message | The mesh is not a closed solid, so the view leaves the boolean out, as Bake would. | Repair the mesh. |
| **"“…” was not refined: … The mesh is unchanged."** | Refine could not simplify, refine or hull the mesh: it is not a closed solid, memory ran out, or the result had no triangles. The Refine panel shows the same reason under its options. | [Repair](Workflow-Repair-a-Scan-for-Printing) a mesh that is not a closed solid. |
| **"“…” was not split: … The mesh is unchanged."** | The cut could not be computed, or pieces joined across a cut you turned off could not be united again. | Read the reason in the alert, which links to the mesh. |
| Bake's status line reads **"Meshed on the CPU: …"** | A GPU meshing method could not run, and the reason follows: WebGPU is missing, the GPU ran out of memory or reported an error, the clay nests deeper than the GPU evaluates, or the mesh needs buffers past the device's limit. | Nothing; the clay is meshed on the CPU instead. |
| A decal or texture map card in Bake shows a reason | Its preview could not be made, or the NanoSVG sampler could not read the SVG. A bake skips that decal with an alert that names the mesh and the SVG. | Fix what the card names. For an SVG that NanoSVG cannot read, choose the DOM sampler under **SVG sampler**. |
| **"Paint could not prepare this mesh for the brush: …"** | Paint could not get a large mesh ready for the brush. | Press on the mesh to try again. |
| A tool's apply button is grayed out | The tool is waiting for something. | Read the status line at the top left; it says what is missing. |
| Combine does nothing | It needs a target and tools. | Select the target first, then Ctrl-click (Cmd-click on a Mac) the tools, then press **C**. |
| The view is offset or lost | The camera points away from the model. | Press **H** to reset the view, or **F** to frame the selection. |
| The panels are rearranged or missing | The panel layout was changed. | Search **Reset Panel Layout** with **Ctrl+K**. |
| **"Preparing shaders — the view may pause briefly"**, at the top left of the view | The graphics card is compiling shaders, and the view can stop drawing meanwhile. | Wait; the notice goes when the compiles are done. |
| A soft fog fills the space where a large mesh will appear | A mesh of 200,000 triangles or more is being prepared for drawing, as after a Bake or an undo. | Wait; the mesh replaces the fog. |

## Files

File problems come from formats Kelcite cannot read, from the browser's memory limit, or from [units](Architecture-Units-and-Files) that differ between the file and the scene.

| Symptom or message | Cause | Fix |
|---|---|---|
| **"Kelcite doesn't recognize this kind of file."** | The format is not supported. | Convert the file to STL, 3MF, OBJ or GLB, one of the formats Kelcite [imports](Workflow-Import-and-Export). |
| **"STEP import is an optional download."** | STEP support is not built into the app. | Open **Downloads**, from the button beside the Asset Library, and download STEP Import. |
| **"It is too large for the memory this browser gives Kelcite."** | The browser limits the memory a page may use. | Close other tabs, or reduce the model in the app that made it. |
| **"It was saved by a newer Kelcite than this one."** | The file uses features this version cannot read. | **About → Check for updates**, then open it again. |
| An import "holds things this version cannot read" | Part of the file uses unsupported features. | The rest was imported. The message lists what was left out. |
| A glTF or GLB comes in with a warning that **Assimp opened it** | Kelcite's own glTF reader could not read the file, so the second reader did. | Check its materials and rig, which may be missing. [Import and Export](Workflow-Import-and-Export) lists the 3MF warnings too. |
| **"Could not import filaments from "…": …"** | The file is not a filament JSON or 3MF, cannot be read, or lists no filaments. | Check the file against the formats [Manage Filaments](Workflow-Manage-Filaments) describes. |
| A banner above the document list says a document **was not exported** | That document's export failed, or it has no saved scene. **Download All** lists every document it left out. | Fix what the reason names, then export again. |
| The model is the wrong size | The file's unit differs from what Kelcite assumed. | Use the model's **Units** row in the Info panel, and check **Files Without Units** in [Preferences](Workflow-Keyboard-Shortcuts-and-Preferences). |
| Export refuses clay, Extrude or texture maps | STL, 3MF and OBJ hold only triangles. | [Bake](Workflow-Bake-an-SDF-to-a-Mesh) them first, or export GLB to keep them editable. |
| An OBJ export downloads several files | OBJ keeps materials and textures in separate files. | Allow the browser to download several files. |

## Storage, privacy and offline use

Files you edit stay on your device, in the browser's own storage, and Kelcite [collects and sends no data](Architecture-Storage-and-Privacy): it has no analytics, no tracking, no login and no cloud storage. Save writes a `.glb` copy wherever you choose. Clearing site data in the browser removes the documents stored there, so important scenes belong in saved files as you go.

When the browser's storage cannot be read, the Asset Library says so in an alert and in a line at the top of its panel, with the reason. The library may then hold assets it does not show.

Kelcite is a progressive web app. Once loaded, it works without a connection, and all processing happens on your computer. Chrome and other browsers also offer to install it locally. Optional downloads, such as STEP import, work offline once downloaded.

Each optional download comes under its own licenses, which **Downloads** lists with links. Where a component's terms differ from the app's, the download waits until you accept them.

## Starting over

Opening [kelcite.app/reset](https://kelcite.app/reset) clears everything Kelcite keeps in this browser: documents, the Asset Library, optional downloads, preferences and the panel layout. **Preferences → Data → Reset everything…** does the same from inside the app. Neither can be undone, so export anything you want to keep first.

To restore only the demo, you add it again from the document list; Kelcite asks before replacing an existing copy.

## Cost

Kelcite is free, with no paywalls, subscriptions, tiers, "Pro" versions, tokens or ads.

## Reporting a bug

Bugs and feature requests go to the [Kelcite Issue Tracker](https://github.com/kelnishi/kelcite-app/issues). A report that includes the Kelcite version, from **About**, and the exact message you saw is easiest to act on. A preview or test build shows the branch and commit it was built from after its version, as in **v0.17.3 · dev@1a2b3c4**; a report from one should include all of it.

## Preparing the scene takes a long time

After 8 seconds the panel offers **Show the scene now**. Objects whose shaders are still compiling stay hidden, with a spinner in the Object Browser, and appear as each is ready. A shader that has compiled for a minute is reported in the notification log with links to the objects it keeps hidden.

## An older Kelcite says a file was saved by a newer version

The scene uses a feature the older version cannot read (Profile clay, a Loft, a closed Wire, or Extrude's taper, twist or path). Open it at kelcite.app.

## A panel says part of the app did not load

A panel whose code cannot load, for example because the browser cleared its storage, names what did not load and offers **Reload**, which saves the open document first. If part of the app cannot load after an update while the page was open and offline, an error names the feature and asks you to reload the page while online.

## A tab was left open during an update

A tab left open while a new version installs keeps working on the version it loaded. Its panels and tools still load and it does not reload by itself; the update banner offers the new version.

## A popped-out panel group is gone after reopening

A panel group popped out to its own window comes back as a floating panel group in the main window when Kelcite opens again. **Pop out** on its header reopens it as a window.

## "Open without drawing the scene?" appears

Kelcite asks only when an earlier session of that document really ended without closing. A document that is already open in another tab no longer triggers it.

## A Loft shows a warning triangle in Objects

The Loft cannot build, or builds a broken body. Hover the triangle for the reasons, which [Loft Between Shapes](Workflow-Loft-Between-Shapes#when-a-loft-cannot-build) lists. Sections that sit on each other, such as the two faces a split leaves, clear once the pieces are moved apart.

## The tour's edits are not kept

The tour starts over each time it opens. Its first edit offers **Save a copy**, which opens as a file of your own, and the tour's name at the top of the window saves a copy too. A tour file kept from an earlier version is removed if it was never edited; one you edited or renamed stays, under an address of its own, without its undo history.

## Clicks in the scene stop selecting

A click made while a Loft's body was rebuilding could leave every later click ignored, while band selection still worked. Kelcite 0.17.390 fixes this.
