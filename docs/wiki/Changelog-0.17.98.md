# Changelog Feature Release 0.17.98

Released 2026-11-04.

Large meshes stay **responsive** through booleans, Extrude, Paint and Repair, and Repair finds **fins and duplicate faces**. Failures name their reason, and Info panel edits undo one step each.

## Clay

Wiki: [Sculpt with Clay](Workflow-Sculpt-with-Clay)

- A body cut, joined or intersected by a single tool shows the tool's color where the tool shapes it; it showed the body's color everywhere unless the body had a second tool or a Texture Map.
- With ground shadows off, dragging clay or a tool that cuts it on a busy or slow computer no longer stops updating the view partway through the drag.
- A mesh turned into clay keeps its sharp edges exactly when its seams' copies of a point differ by rounding noise. A mesh with a face wound the wrong way reads its samples instead of exact edges there, whose inside and outside it could swap.
- A displaced or lattice clay body keeps showing its last picture while an edit's shader compiles, instead of freezing the page until it is done.
- Hair, Skeletonize and mesh clay start their background task again after it stops or runs out of memory, instead of their previews waiting forever.

## Modifiers

Wiki: [Modifiers](Tool-Modifiers)

- A mesh cut by a boolean modifier shows its new triangles after an edit that keeps its points, such as flipped faces, and its new face groups after painting them; it used to keep showing the result from before the edit until something else changed. The same holds for a cutting tool's edited triangles.
- Moving a mesh's boolean tool on a large mesh no longer freezes the window while the cut rebuilds; the previous cut stays drawn until the new one is ready.
- A mesh's Difference, Intersect and Union show the result Bake will write, face for face, since both now compute it the same way. A boolean on three spheres of 1M triangles each previews in about a quarter of the time it took, and moving a boolean's tool rebuilds the cut faster at every size.
- A boolean on a mesh that is not a closed solid is left out of the view, with a message saying why, as Bake would leave it; the view used to show a cut that Bake then refused.
- A mesh's boolean or Extrude result draws its materials, face groups and flat shading from one copy of the result, so a large result takes less graphics memory and switching flat shading or Show face groups on it shows at once without rebuilding.

### Array

- An array looks the same whatever Up Axis is set in Preferences, which only says how files are read and written. Arrays stand in the scene's own frame, where up is Y, so the demo's dice turn about the vertical.
- An array made while Up Axis was set to Z can open with its copies tipped a quarter turn onto their side, and an Upright one now stands along the vertical; Flip and Turn under Orientation stand the copies back.

### Booleans

- The faces a Difference cuts, a Union adds or an Intersect keeps from the tool show the tool's material, and Bake and export keep it there, added to the mesh's materials; they used to take the mesh's own material.

### Extrude

- Dragging Depth or Inset on a large mesh keeps up with the pointer, and the view draws each result the drag builds. The Wall Groups rows on the Extrude card, in the Info panel and in the Hair inspector update when the drag ends.
- Baking an Extrude on a mesh with seams, or on an STL, gives the same result as the preview. The bake used to inset and wall the raised faces along every seam, and along every triangle of an STL.
- Dragging Depth on a large mesh holds less memory while the slider moves, about 450 MB less at 1.2 million triangles, and the view draws more of the drag's steps.
- Dragging Depth on a large mesh redraws the extrusion every frame without rebuilding the mesh, as long as the extrusion touches nothing else; letting go builds the final mesh once.
- Dragging Depth through 0 no longer redraws the whole mesh at 0, only the extruded faces.
- Scrubbing Depth across 0 on a large mesh no longer stalls at each crossing; at 1.2M triangles the slowest tick went from about 0.75 s to under 0.3 s.

### Masks

- Choosing a mask target whose image cannot be read still sets the target, and now warns, with a link to it, why the mask kept its source.

### Skeletonize

- On clay, a lattice preview for a setting already passed stops building when the setting moves on.

## Bake

Wiki: [Bake an SDF to a Mesh](Workflow-Bake-an-SDF-to-a-Mesh)

- Raising Target triangles, or any setting that makes the preview larger, draws the larger mesh's wireframe at once; it used to keep the smaller mesh's lines over the larger one, or show none, until the preview changed several more times.
- When the GPU runs out of memory or reports an error while meshing clay, the status line says why and the clay is meshed on the CPU, instead of coming back empty with no reason.
- The first bake or preview of clay on a GPU method takes far less GPU memory: about 410 MB at 1.2 million triangles, down from 1.3 GB.
- When a GPU meshing method is meshed on the CPU instead, the status line says why: WebGPU is not available in the browser, the clay nests deeper than the GPU evaluates, or the mesh needs buffers past the device's limit.
- Bake and Export: a texture map or color map whose image holds no pixels fails with that reason, in an alert for Bake and in the Export dialog, instead of skipping the map.
- Choosing marching cubes or surface nets in Bake on a first visit no longer freezes the view for about 1.6 s while the GPU compiles their shaders; the first preview appears about 0.8 s sooner, and mesh refinement passes compile ahead the same way.
- A mesh with a flipped face or an extra fin is baked as an open mesh, like one with holes, instead of failing the bake.
- A decal or texture map whose preview cannot be made shows the reason on its card instead of leaving the mesh unchanged without a word.
- When the NanoSVG sampler cannot read an SVG, the decal's card shows the reason and a bake skips that decal with an alert that names the mesh and the SVG as links. Before, the decal was made with the DOM sampler although NanoSVG was chosen.
- The first preview of a session draws in about half the time, and the scene keeps drawing while Bake gets its graphics work ready, instead of freezing for seconds with a wireframe toggle or other changes not showing.
- Refine and Bake: with many parts selected, previews no longer search the whole scene once per part on every frame; 600 selected parts of 3,000 drew at 46 ms a frame and now draw at 17 ms.
- Dragging the cell size slider on a clay stops the preview of the earlier position at the mesher's next step and keeps the GPU mesher for the next one, so the preview of the position you stop on appears sooner.
- With dual contouring, the clay preview updates in about two-thirds of the time after a cell size change, and dragging the cell slider slowly shows each step's mesh instead of only the last.
- With smoothing or decimation on, dual contouring's mesh can differ slightly from an earlier bake of the same clay; bakes without them are unchanged.
- Patch and union caps two holes that meet at a single corner with two caps.
- The preview of clay made from a mesh redraws sooner after a setting change, about a quarter faster on a 100k-triangle mesh, because its meshing stays loaded while Bake is open.

### Refine

- When a mesh cannot be simplified, refined or hulled (a mesh that is not a closed solid, memory run out, the worker stopping), the panel shows the reason under the options, and Accept leaves the mesh as it was with an alert that links to it and gives the reason. A result with no triangles is reported the same way.
- Dragging the Simplify tolerance on a large mesh updates the preview sooner, and the last preview stays on screen while the next one computes.
- In Chrome and Firefox, the vision model runs on up to four CPU threads when the browser has no WebGPU, where it ran on one.
- The smooth-shaded preview of a mesh without face groups shades across seams where its vertices are split, as the viewport does.
- After a slider drag, the preview of the value you let go at shows sooner (on a 400k-triangle mesh, 0.5 s after the last value where it took 1.3 s), because the previews of values passed during the drag stop instead of finishing first.
- Applying writes exactly the result the preview shows, and applying while the preview is still computing waits for it instead of computing it a second time.

## Split and Combine

Wiki: [Split and Cut a Model](Workflow-Split-and-Cut-a-Model)

### Split

- When a mesh cannot be split, it stays as it was and an alert names it, with a link, and gives the reason. This covers a cut that cannot be computed and the pieces joined across a cut you turned off that cannot be united again. Before, Split left the pieces apart in the second case and said nothing in the first.
- A mesh with a few triangles wound inside out is cut, where Split refused it as not manifold; the pieces are wound outward.

### Combine

- A boolean of large meshes builds in the background, so the app stays responsive while it works. Three spheres of 1M triangles each used to freeze the window for about 20 seconds.
- A progress bar shows which step a large boolean is on, and closing Combine stops it at once.
- Commit keeps the result the preview built, so it finishes at once instead of pausing for about a second on large meshes.

### Part

- Parts that touch only at a corner, along an edge or across a face are separated into parts of their own, and a fin standing off a solid becomes a part of its own. Before, any two triangles that shared a corner stayed in one part.

## Trace

Wiki: [Construction and Rigging](Tool-Construction-and-Rigging#trace)

- Tracing a face group's outline gives one closed trace per loop where two of its regions touch at a corner.
- Splitting a mesh with seams, or an STL, along its traces splits the faces on both sides of a seam alike, so painting the regions afterward no longer fails.
- A trace snaps to the mesh's edges across seams and on STLs, and the regions it paints stop at the trace, not at every seam. A trace that no path of mesh edges can follow stays floating instead of reading as split.
- Loop Type in the Info panel reads Closed (with siblings) only when the traces that meet it lead round to its other end; a trace that merely touches another reads Open (with siblings).
- Wire and Trace: picking a pin or a wire no longer tests every triangle of the large meshes in the scene.
- Trace, Crack and Pin read a mesh with an edge shared by three or more faces the same way every time it loads: the faces that close each solid are joined, and a fin is left as an open edge.
- Applying a trace on a mesh with an edge shared by three or more faces removes the faces left over there (a fin, a face stored twice) and keeps two parts that meet along an edge closed, the same way every time the mesh loads. Before, it removed whichever faces were stored last on that edge, which could open a hole in a part.

## Pin and Wire

Wiki: [Construction and Rigging](Tool-Construction-and-Rigging)

- Wires and other line overlays: moving a point a wire hangs from no longer makes the next frame wait on a shader compile, which stalled the view at the first edit after a cold load.
- Pin, Trace, Crack, Jig and Scribe: right after an edit to a large mesh, moving the pointer over it no longer freezes the view while the mesh is prepared for picking. The preview under the pointer appears once it is ready, and a click made meanwhile takes effect then.

## Move

Wiki: [Construction and Rigging](Tool-Construction-and-Rigging#move)

- A body dragged with surface snap over a body it cuts or joins lands on that body's surface as it is without the cut, instead of diving into the hole it makes. Drop to ground's Surface below and Stack's Surface contact land the same way.
- Drop onto the body below and Arrange no longer reorder the triangles of a mesh they land on, which left its face groups, materials and paint on the wrong triangles.

## Repair

Wiki: [Repair a Scan for Printing](Workflow-Repair-a-Scan-for-Printing)

- A new row lists fins and duplicate faces, the faces left over where three or more faces share an edge. Remove fins and duplicate faces removes them when you accept, and Repair All includes it. A fin's open edges are no longer listed as a hole.
- Repair keeps the texture coordinates and vertex colors of the faces it keeps.
- The "not manifold" notice for a mesh offers Repair now when it has fins, duplicate faces or faces wound against their neighbors: it removes the extra faces and winds every face outward, in one step you can undo. Importing a file never changes it on its own.
- The "not manifold" warning counts the mesh's open edges, edges of three or more faces and pinched corners with its coincident corners joined, so its numbers match the Repair panel's: an STL with one fin reports 2 open edges where it reported more than 24,000.
- Flipped faces are found part by part. The inner wall of a hollow part, which faces into its cavity, is no longer reported as flipped, and Orient winding turns each separate part outward on its own. A part with a small hole that is wound wholly inside out is reported as flipped; a sheet, or a part open across a quarter of its surface or more, is left as it is wound.
- A hole's patch is wound to match the triangles around it by the edges it shares with them.
- Accepting fixes keeps the mesh's face groups and their materials. Before, a mesh with face groups came back with none, or with only the groups of its new patches. A patch that gets a face group of its own takes the next group after the mesh's.
- A repaired mesh takes about a third of the memory it did, and saves smaller, because its triangles share their corners instead of each storing its own.

## Crack and Scribe

Wiki: [Surface and Refine](Tool-Surface-and-Refine)

- Crack, Pin and Texture Map: walks and paths along a mesh's edges cross an edge next to a face wound the wrong way from either side, where they could stop.
- Crack and Repair: a curved cap continues the surface around its hole the way the surface faces. On a piece that curves toward its outside, such as the inside wall of a cup or a seat's hollow, the cap used to bulge the wrong way.

### Crack

- Opening a mesh with face groups shows every boundary between two groups as a crack, however long. A boundary of more than 10,000 edges, as on a large scan, used to be left out.
- Two regions that touch at a single corner keep their own cracks and their own caps, the same whatever order the mesh's faces are stored in. They could merge into one figure-eight crack, and one cap across both holes.
- A triangle wound inside out no longer splits off the faces around it, so a mesh with a few flipped triangles groups as its clean copy does. A face stored twice, or a fin standing off a solid, is its own group, and two parts touching along a face keep their groups apart.
- The caps Part mode closes each piece with are wound as the piece is along every edge they meet it, where some cap triangles faced inward.

### Scribe

- The boundary lines follow the mesh after an edit that keeps its face count, instead of staying on the shape it had before.
- Crease lines show on an STL import, where none were drawn.

## Rig and Pose

Wiki: [Rig and Pose a Character](Workflow-Rig-and-Pose-a-Character)

### Rig

- New rig stands the rig upright in the scene, where up is Y, whatever Up Axis is set in Preferences, which only says how files are read and written. A rig made while Up Axis was Z stays as it was, lying along the scene's Z; Move turns its group to stand it up.
- Automatic rigging treats parts that touch along an edge or across a face as separate parts, as Part does.

## Paint

Wiki: [Paint Decals and Texture Maps](Workflow-Paint-Decals-and-Texture-Maps)

- On a mesh with a Difference, Intersect or Union modifier, the faces the cut leaves of the mesh take the color you paint, as you paint it, and keep it after Undo and Redo; they used to keep their old color until something else changed.
- On a mesh with an Extrude of negative depth, the ghost of the result now shows the cut. It drew behind the mesh's own surface before, so none of it was visible.
- Texture Map and Decal: a wrap is solved again after any edit to its mesh or its face groups, such as Orient turning a few faces. An edit that kept the mesh's size could leave the wrap laid out for the mesh before it.
- Undo now removes a color added with Shift+Plus; the palette comes back as it was.
- A mesh with a boolean shows its result while Paint is open, the faces its tools cut or add in the tools' materials. It showed nothing before.
- Painting a BitCanvas onto a large mesh it displaces follows the pointer without the pause each move had. The brush reads the mesh's smooth normals as the viewport draws them.
- Opening Paint on a large mesh takes about a tenth of the memory it took, about 110 MB instead of 1.1 GB on a 5-million-triangle mesh, and the brush is ready in about a third of the time.
- Closing Paint gives back the memory it used for the brush.
- Letting go of a brush stroke on a very large mesh no longer stalls the view for a moment. The stroke is stored and drawn a few milliseconds at a time, and the first stroke on a mesh with many face groups draws without a long frame.
- While the ghost of the mesh's modifiers is hidden (Shift held, or Ghost Opacity at 0), a stroke no longer rebuilds the modifiers behind it. The ghost updates once when it shows again, and stays hidden until it matches the mesh.
- A brush stroke on a large painted mesh now shows as soon as you let go. The mesh updates only the faces the stroke touched instead of building and uploading the whole mesh again, and the stroke's preview no longer stays on screen while the mesh catches up. Undo and redo of a stroke update the same faces.
- The result of a mesh's boolean, shown while you paint, is now built in the background after each stroke instead of pausing the view.
- The brush and the fill spread across the triangles of an STL and across seams where a mesh's vertices are split, as they do on a mesh made in Kelcite. Before, the fill and the brush stopped at the edge of every triangle of an STL.
- When the background task that draws a mesh's modifiers stops or runs out of memory, the next stroke starts it again, instead of the mesh's preview no longer updating. Crack shows an error instead of waiting forever.
- When Paint cannot prepare a large mesh for the brush, it says why in an alert and tries again the next time you press on the mesh, instead of preparing it on the main thread, which held the app for the time it took.
- When the ghost of the mesh's modifiers updates slower than the FPS threshold (20 fps, a 50 ms update) during your first brush stroke, it stops updating while you paint. It hides for each stroke and updates once when you let go, and stays that way until you close Paint. Meshes whose ghost updates quickly keep updating during strokes.

### Decals

- Hovering a decal modifier on a large mesh draws the outline of its projection without the pause it had, because the outline now takes half the ray casts and each goes through the mesh's triangle index.
- Where the wrap cuts a seam through a mesh's own seam or an STL, each side of the cut stays with the faces it belongs to.

### Texture maps

- Displacement keeps a mesh closed along its seams and on STL files. Each point of the surface moves once, along the normal of every face around it, where seams and STL triangles used to pull apart into cracks.
- Displacement and normal maps keep a mesh's hard edges. Where a mesh stores a different normal on each side of an edge, each side keeps the normal of its own faces, and the edge stays closed.
- Edge Blend measures across seams and STL triangles, so a wrap or a projected map no longer flattens to nothing along every seam, or over a whole STL.
- Normal maps give every copy of a seam vertex the same normal, so the shading no longer breaks along seams.

## Objects and the interface

- Repair reads "1 Hole", the Pose panel reads "1 clip", and Auto Setup Filaments reads "Create 1 filament" for a count of one; each language uses its own plural forms.

### About

- A preview or test build shows the branch and commit it was built from after its version, for example "v0.17.3 · dev@1a2b3c4". kelcite.app shows the version alone, as before.
- A fix released between two versions has a fourth number, as in 0.17.3.1, and Check for updates offers it like any other version.

### Camera

- With Re-center the pivot after panning on, the pivot moves to the body at the center of the view when a pan ends, and the diamond lands there, instead of at the start of the next orbit. On a large mesh the brief pause this takes now falls between the pan and the orbit rather than at the orbit's press.
- A 6DoF controller never re-centers the pivot. Its slides no longer count as a pan, and a mouse pan that ends while the controller is moving keeps its pivot.

### Asset Library

- When the browser's storage cannot be read, an alert and a line at the top of the panel give the reason, so an empty library no longer looks like an empty library.

### Undo

- Changes made in the Info panel now undo with one step each, and a slider drag undoes as a whole. These cover material and volume colors, SVG and text settings, image settings and palette, DPI, anchor rotation, primitive settings, and wire tangents.
- Adding, renaming, hiding and removing a layer, renaming an object in the Object Browser, and Solve in the Setup Table now undo.

### Objects

- Undo now reverts Recompute normals, in one step for every mesh selected, and Redo recomputes them again. Before, Undo skipped past it and reverted the edit before it, and the old normals could not be recovered.
- Dropping an image or SVG on a large mesh's row places it without the pause it had.

### Selection

- Clicks, hovers, the Paint brush and the orbit pivot pick a large mesh through the graphics card instead of a search structure the app built for each mesh, so a large mesh no longer waits a second or more after it is drawn before it can be picked, and a posed character is picked where it stands without a pause after each pose.

### Info panel

- Dragging a large mesh with the Info panel open no longer recomputes its size on every frame; only a rotate or a scale does.

### Materials

- Separate disjoint regions counts a material's faces on two parts that touch along an edge or across a face as two regions.

### Preferences

- In General, under Tools, "Suspend slow updates during paint" turns that off, so the ghost keeps updating during strokes, and "FPS threshold" (1 to 60) sets how slow an update must be to suspend it. Turning the toggle off also ends a suspension already in effect.

## Files and the tour

Wiki: [Getting Started](Workflow-Getting-Started#the-demo-tour)

- Export shows its shortcut, Ctrl+E (Cmd+E on a Mac), again; keybindings saved by an earlier version that had turned Ctrl+E into a second Save get Export back.
- 3MF export lists filaments in Extruder # order, so editing a profile's Extruder # changes its slot in the slicer.
- Auto setup continues the Extruder # numbering after the profiles a document already has, and a new or imported profile takes a free number.
- Importing a model 3MF adds its filament profiles to the document, so its materials link to profiles that exist; a profile the document already has is reused.
- Auto Setup Filaments starts within its slider's range (at most 16).
- Import filaments names the reason in an alert when a file cannot be read or lists no filaments.
- FBX and OBJ export write the normals an imported file brought as they are, until an edit changes the mesh or its normals, where OBJ replaced them with computed ones. Every other mesh gets the normals the viewport shades with: a vertex between two face groups takes each group's normal. A saved document keeps computed normals, so a mesh reopened from one exports the viewport's.

### The tour

- The tour's Repair stop now shows a fin, a face sticking out where three faces meet at an edge, as well as the open top, so the Fins and Duplicate Faces row appears beside the hole.
- The tour no longer has stops for the performance overlay, the performance guard or the camera pivot. The overlay, the guard's settings and the pivot diamond work as before.
- Hiding the tour leaves nothing over the view; the demo node's Info panel has a Show demo button to bring the controls back.
- The Move quick edits stop is removed. The Move tool's Body section exists only while the Move tool is open, and an open tool hides the tour, so the stop could not point at it. The Move tool's quick edits work as before.

### Documents

- A failed GLB, 3MF, STL or OBJ export, a document without a saved scene, and a failed Download All now show a banner above the list that names each document and gives the reason. Download All lists the documents it left out of the archive, and makes no archive when none can be exported. Before, these exports ended without a word.
- Opening a document with clay no longer freezes the page for seconds at a time on a first visit, while its shaders compile.

### Export

- Files weld corners that coincide by rounding their coordinates to the nearest 0.0001, instead of cutting off the digits past it, so two copies of one point that differ only in the last digit become one vertex. An exported glTF file can have a few vertices fewer; STL, OBJ and 3MF files of Kelcite's own models come out as they did.
- A rigged model's GLB shades smooth across texture seams, and an STL's rigged mesh shades smooth instead of faceted. Its normals are the ones Recompute Normals stores.

### Import

- A glTF or GLB file that Kelcite's own reader cannot open, and that the second reader (Assimp) opens, now comes in with a warning that gives the first reader's error and says Assimp opened the file, since its materials, rig and extensions may be missing.
- A 3MF whose model part cannot be read fails with the parser's reason, including a part another part refers to, which used to import without its objects. A 3MF that runs the browser out of memory while it is scanned reports that, and does not retry in a way that cannot succeed.
- A 3MF whose component names a part that is not in the file opens without that component, and warns with the part's path and the object that referred to it; it used to skip the component without a word. A file with nothing else in it fails with the missing path in its reason.

## Rendering and performance

- A notice, "Preparing shaders — the view may pause briefly", shows at the top left of the view while shaders compile. The 3D view can stop drawing during a compile, so the notice says why. It appears only after a compile has run for about a tenth of a second, and goes when the compiles are done.
- A mesh of 200,000 triangles or more that appears in the scene, as after a Bake or an undo, no longer holds the app while it is first drawn: a soft fog fills the space it will take until it appears. At 5 million triangles the pause was about half a second and is now a few hundredths of a second.
- Focusing on a mesh that is still appearing frames it once it has appeared, on its own bounds.
- Editing the shape of a large mesh (a Repair, a transform bake) no longer holds the app while its shading is recomputed: the mesh keeps showing until the new surface is ready. At 5 million triangles the pause was up to 1.5 seconds and is now under a tenth of a second.
- Opening STL, STEP, FBX and other files, mesh analysis, segmentation, Refine, Split, Paint on large meshes and package downloads start their background task again after it stops or runs out of memory, instead of waiting forever.

### Wireframe

- A large mesh's wireframe wipes in across the model, corner to corner over a quarter second, when its geometry finishes building; a wireframe switched on over a built model appears at once, and so does one with Reduce motion on.
- A large mesh's wireframe draws each edge once and builds without freezing the view when you switch it on; switching it off gives its memory back.

### Viewport

- Pressing the right or middle button, or turning the mouse wheel, no longer pauses the view on large meshes before the camera moves. At 1.2M triangles a right press held the view about 115 ms, and each wheel tick about 60 ms.
- Clay with a displacement map no longer blanks while a body that cuts it is dragged or while its Scale slider is scrubbed; the last shape stays on screen until the next one is ready.
- Dragging a body that cuts a mesh no longer stops updating the view after about a second.
- Clay stays on screen when a Skeletonize lattice first arrives or an Array is switched on, instead of vanishing while its new shader compiles.
- Selection boxes are bright cyan around one body and green around a group, in light and dark mode, instead of dark navy and dark green; the preview box a quick edit shows takes the same cyan.
- The wireframe and other overlays switched on early in a session draw within a moment, instead of waiting seconds behind the scene's shaders as they compiled.
- On large meshes, tool previews no longer pause the view when a pan ends or a click lands on them. Bake's preview, the halves Split shows, Refine's result, Part's pieces, Scribe's segments, clay drawn as a mesh and the copy a tool works on are now picked through a triangle index, as meshes already were.
- Turning it on for a mesh without face groups, or for the result of a modifier stack, shows faster and uses a quarter of the graphics memory it used; at 5 million triangles it takes about 0.1 s, where it took 0.6 s.
- A painted mesh, a mesh of several materials, and Show face groups now draw from one copy of the mesh instead of a separate copy for each look, so a large painted mesh takes far less graphics memory, and Show face groups, flat shading and hovering a face group show at once without rebuilding the mesh.
- The first right-click on a large mesh no longer holds the page while the mesh is prepared for picking. On a 5M-triangle mesh the page stood still 1.3 s and now about 0.4 s; at 1.2M triangles the face-group menu opens in about 0.14 s instead of 0.39 s.
- On meshes of a million triangles or more, a click, a tool's hover and the camera's re-pivot after a pan no longer hold the page. A click on a 5M-triangle mesh took about 0.7 s and now takes under 0.1 s; a pan's release, an orbit about the point under the cursor and Pin's hover went from about a quarter second to one frame.
- In the second or so after a large mesh first appears or changes, a click, a hover or the camera's re-pivot on it waits until the mesh is ready to be picked, then takes effect where it was made.

### Large meshes

- An edit that only moves a large mesh's vertices, such as Repair or applying a transform, now redraws it without rebuilding the whole mesh, and a large mesh holds less memory while it is shown.
- Turning off Show face groups or flat shading gives back the memory the look used, about 0.6 GB for Show face groups on a 5-million-triangle mesh. After trying every look and turning them off, the page holds about 1 GB less than before.

### Memory

- Editing a large mesh again and again no longer leaves each earlier version's display data in memory, and Scribe's cached segmentation data is dropped when the mesh changes.
- A worker whose WASM heap grew past 512 MiB is ended after 60 s idle, which returns that memory; its next job starts a fresh worker.

### Performance overlay

- Lists each worker's memory pools and WASM heap peak, and states when a worker was restarted and why.
- Frees unused memory pools on the page and in the workers before it warns about JS memory or GPU buffers, and warns only if the measure is still over its limit.
