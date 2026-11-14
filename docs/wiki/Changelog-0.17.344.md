# Changelog 0.17.344

Released 2026-11-12.

Clay gains a **Profile** shape that extrudes outlines, **Loft** builds solids through sections with matching pins, Extrude gains **draft, twist, bands and paths**, and Split cuts along a **Trace** with clean caps.

## Clay

Wiki: [Sculpt with Clay](Workflow-Sculpt-with-Clay)

- A new Profile shape extrudes an SVG, text, closed Trace or closed Wire as clay. Pick the outline in the Outline row, set the Height, and add a Draft angle or an End scale and a Twist; the clay follows the outline when it changes, and Round, Threshold, Combine and Bake work on it as on any clay.
- Polygon gains a Twist in the Inspector, which turns the section about the shape's up axis over its height.
- Polygon is now one of Profile's three shapes, Polygon, Star and Outline, in the Clay panel and in the Inspector. Choosing Profile starts on a five-sided Polygon; Outline extrudes an SVG, text, closed Trace or closed Wire as before. Clay bodies saved as polygons or stars open as Profile's Polygon or Star, unchanged.
- Every Profile shape has the same Taper, Twist and Corner radius, below its own settings. Taper is a Draft angle or an End scale on all three; a Polygon's or Star's Draft leans its walls by that angle at any size and keeps it when the body is resized, and its End scale now reaches 2 for a wider top.
- A Star sets its Inner radius, and in the Inspector a Polygon or Star sets its Radius and Height directly.
- An Outline read from an SVG or text with several paths lists them with checkboxes. Check one path, several, or all; several join as one outline, a hole stays a hole while the path around it is checked, and hovering a row lights that path in the scene.
- An SVG path's holes now cut through an Outline as they show in the SVG. Outlines made earlier from SVGs or text whose paths have holes (an "O", an "e", a ring) change when their document opens: the holes they had filled are open.
- An Outline's Outline strokes switch decides whether the SVG's strokes count, outlined the way the SVG's own Outline strokes outlines them. On, the default and what existing Outlines read, a stroked open path can be checked; off, the Outline reads the SVG's fills alone.
- Switching between Polygon, Star and Outline keeps the height, twist, taper, Corner radius and Round modifier, and switching back restores the shape's own settings.
- A baked Profile has a face group for each wall (each side of a Polygon, each facet of a Star, each run of an Outline between corners) and one for each cap, with Twist, Taper, Corner radius and a Round modifier as well, so Paint, Shell and Trace can pick a wall or a cap's rim.
- An Outline follows its source when the source is edited: reshaping the SVG, text, Trace or Wire it reads updates the clay at once, and moving, turning or tilting an SVG or text carries the clay with it, standing on it as it was, with no rebuild and no lag behind the drag.
- Corner radius rounds a Profile's corners in its plane before it is extruded: every corner of a Polygon, a Star's points (its inner corners stay sharp), an Outline's convex corners, keeping its size. The Round modifier, which rounds every edge of the body, stays a modifier of its own; the two no longer change each other.
- An Outline's selection box, measures and Size are its body's, through its taper and twist, instead of a 1 mm cube; a Polygon's or Star's grow where a wider top or a Draft reaches past its box. An Outline's Round modifier slider reaches as far as the body is large.
- The × in an Outline's source picker clears the source: the Outline keeps its place and every setting and draws nothing until a source is picked, and the Inspector says so. Picking the same source again brings the body back on it where it now is, as it was; picking another keeps the clay where it is and shows the new source's shape there. A source that was deleted can be picked again from the same picker.
- An Outline made from an SVG or text stands on it at no offset: the clay takes the SVG's position and axes, raised half its height, so the body lies over the SVG's shapes path for path, and reading other paths or turning Outline strokes on or off changes its shape and never moves it. Twist, Draft and End scale act about the SVG's center whatever the SVG and the clay have been through; moving the clay moves it off the SVG, and the SVG then carries it from there.
- Opening the Clay tool with one SVG, text, closed Trace or closed Wire selected opens Profile → Outline on it, with the body previewed on it; Create makes the clay there. With nothing like that selected, or several nodes selected, Clay opens as before.
- Commit in the Inspector's Units row on a model holding an SVG and its Outline keeps the Outline on the SVG.
- Sculpted clay bodies no longer fade to 90% opacity while an operation on them runs.
- Clay, Refine, Skeletonize and Paint: A body the open tool is updating no longer shows the pending wave or a row spinner while its drawing is rebuilt, because the tool's own preview effect already marks the wait. Undo, redo, committing a tool and edits in the Inspector still show the wave for a slow update.
- With the Clay tool open, a clay made from a mesh shows a violet grid at its field's cell while the field rebuilds past 300 ms.
- A clay body drawn for the first time no longer freezes the view for seconds. Displaced clay, clay with decals and mesh clay compile their shader in a fraction of the time (a displaced clay with a decal, 17 s before, now opens in about 2 s), and the demo opens with its first view in about a third of the time.
- Displaced clay draws 12 to 22 percent faster per frame; plain clay and decals draw as fast as before. The picture is unchanged.
- Convert to clay on a mesh of a million triangles shows the clay in about a second, where it took about 20 s, and at 5 million triangles in about 2 s, where it took over a minute, since the mesh's distance field is now built on the GPU.
- Adding a Round to a mesh made clay takes about 3 s on large meshes, where it took 10 s at a million triangles and 25 s at 5 million.
- Where the browser has no WebGPU, or the GPU cannot hold the mesh, the field is built on the CPU as before, and the Info panel says so and why.
- Round on a mesh made clay takes about 0.6 s on a mesh of a million triangles, where it took 2.6 s, and about 1.4 s at 5 million, where it took 3.7 s. Each step of the Round slider takes about 0.3 s, where it took 2.3 s at a million triangles and 2.8 s at 5 million.
- Converting a large mesh to clay, or rounding it, no longer freezes the view while the clay appears. The mesh stays on screen, marked as updating, and the clay replaces it once its data has reached the graphics card.

### Displacement

- Blurring a large height map holds half the memory it did, and a stack of masks on one image copies the image once, so a 4096 by 4096 map no longer needs an extra 64 MB per blur.

### Round

- Dragging the Round slider on mesh clay of a million triangles redraws about 2.6 times faster than before, and the rounded clay is the same as before.
- The slider no longer saves each value it passes through to the document's stored clay; the value where it rests is saved about a second later, so reopening the document finds it.

## Loft

Wiki: [Loft Between Shapes](Workflow-Loft-Between-Shapes)

- Every face group's menu now has Loft, which traces the group's outline and starts a Loft from it. It is in the Materials rows of the Info panel, in the right-click menu on a face (a long press on a touch screen), and beside each picked group in the Trace tool.
- The Info panel's Loft on a Materials row now traces that material's face group; before, a row whose material number differed from its group's took another group's outline.
- Add match is a "+ Add match" pill like "+ Add Section", each match's sections are named separately, and a Loft whose matches cross says that no body builds instead of "Building the body…".
- The card of a Loft on a Trace has Combine into, named for the mesh the Trace is on. It joins the body to that mesh as a Union (a boss or a handle) or a Difference (a pocket or a tunnel), and Bake on apply bakes the result as Combine does.
- Where the Trace is on the mesh, the join replaces the mesh's faces inside the Trace with the body and shares the Trace's vertices, so the result is one closed solid with no gap or doubled wall at the Trace. Every other triangle of the mesh keeps its place, group and material. On a mesh of a million triangles the join evaluates about four times faster than the same cut through the Boolean.
- A Trace that lies on the mesh's surface but not on its vertices, such as a smoothed or projected one, now closes its end on the mesh's faces cut along the Trace instead of capping flat, and joins the same way. Project onto surface is needed only for a Trace that is off the surface.
- A body that cannot use the Trace joins through the ordinary Boolean, and the card says why: a Loft baked to a mesh, a body that meets the mesh somewhere besides its Trace, a body on the other side of the surface from the operation, or a modifier ahead of the Boolean. After an edit to the mesh, the join waits for the body to rebuild on the edited mesh and then welds again.
- Matches in the Loft card say which point of one section meets which point of the next. Add match, then a click on each section's outline in turn, places a match; Done or Escape ends it once it has two points. A pin on a star's tip and another on a square's corner give the vase the turn between them, and a pin at 90° on the second section is a 90° twist.
- A pin drags along its outline with the card open, and each drag is one undo step. Pins are orange dots with their number, the match being placed or hovered in its row draws in magenta, and the tracks between the sections draw in blue.
- Two or more matches on a pair of sections split both outlines into spans at the pins, and each span maps onto its partner; a span with the same number of corners on both sides pairs them in order, so a square stays square. Turns adds whole turns that still end at the pins, for a corkscrew.
- Matches that would cross are refused. The card names them ("Matches 2 and 3 cross on Circle"), draws red rings around their pins, and the Loft builds nothing until one moves.
- A match fixes its pair of sections, so Start and Twist leave those pairs alone and say "Set by match 1." Pins follow a section that moves and snap to the nearest point of an outline that is edited, and a match keeps its sections through a reorder.
- A Trace, an SVG or text, a closed Wire or a Pin can lead a Loft, which builds a solid from that section through the next ones in order. The solid is a part of its own: it draws beside the node that carries the Loft, exports as a separate part, and Bake to a mesh turns it into a mesh that moves on its own.
- Trace a face group, then loft it. Loft from this group, in the menu of a face group in the Info panel, traces the group's outline and puts the Loft on the new Trace. Add Loft in the tool menu's search, or in the Add Modifier menu with several nodes selected, takes the selection as the sections in order and starts from a Trace.
- The card lists the sections, each with buttons to move or remove it, and Add Section picks the next one. Blend is Ruled or Smooth, Twist turns each pair of sections against each other, and Tolerance sets how finely a curved span is cut into rings.
- Each section leaves along its take-off, which is Auto (the direction its outline faces) or a Pin you aim. Strength sets how far it keeps going before it bends toward the next section, and Start turns where one section's outline meets the next.
- A Trace on a mesh's edges, as a face group's outline is, closes the body's end with the mesh's own faces under the loop, so the body meets the mesh with no gap. Region chooses the smaller or the larger side of the loop. A Trace that has left the mesh's edges closes flat, and the card says how far off the surface it is; Project onto surface moves it onto the mesh.
- The body follows its sections and nothing else. Moving the part that carries a Trace and the other sections moves the body without building it again, and an edit to the mesh under the loop rebuilds the end. Dragging Twist on a mesh of a million triangles keeps the view at 60 frames per second.
- Right-clicking a Loft's body in the scene view without dragging, or holding a finger on it, opens a menu with Bake to a mesh. A host's row in the Object Browser has the same entry.
- Add Section also takes a face group. Click a mesh in the view (the group under the pointer lights up) or open the list and pick a group under its mesh; the group's outline is traced as a Trace, added as the Loft's next section and selected, as one undo step. A group with no outline, or one that is already the Loft's own Trace, says so.
- Choosing Loft from a face group's menu (the right-click or long-press menu on a face, or a Materials row in the Info panel) now leaves the new Loft waiting for its destination. The Loft card's Add Section is on and reads "Select target section", so the next click or tap on a face group (or on a Trace, closed Wire, text, SVG or Pin) adds the second section. A click on the face group the Loft starts from says it is already a section and keeps waiting. Escape, Cancel beside the status, or pressing Add Section again leaves the Loft with its one section.
- The walls divide into a face group between each pair of tagged corners that meet from one section to the next, so a Loft from one box face to another gets four walls, plus a Start cap and an End cap. A section without tagged corners divides nothing.
- An SVG, text or Wire section has a Tag corners for face groups button on the Loft card. It tags the corners where the outline turns by more than 40°, which show as orange diamonds while the Loft is selected, and pressing it again clears them.
- A Groups section on the Loft card lists the walls and caps and gives each its own color. Bake to a mesh gives every group a material named Wall 1, Wall 2…, Start cap or End cap, so Paint and the Info panel can address it.
- Bake to a mesh checks the new body (manifold, topology, weld) and keeps it spinning in the Object Browser until that check ends; a check that fails stops the spinner and says why.
- A Loft's body now draws the wireframe (Alt+W) and, with X-ray also on for its Trace, wires only, as meshes do. Before, the body stayed solid.
- A Trace, Wire or SVG that carries a Loft shows the X-Ray toggle in the Object Browser, and Toggle X-Ray (Alt+X) reaches it; X-Ray applies to the Loft's body, and with the wireframe on the body draws wires only.
- A Loft's body now follows its sections as you move them. Moving a section's Trace or SVG with the Move tool, editing a Trace's points, and Undo or Redo of either redraw the body at once; before, it stayed where it was until something else changed.
- A face of a baked platonic solid (tetrahedron, octahedron, dodecahedron, icosahedron) can be a Loft section; its outline no longer runs out and back along the crease, which left no closed loop to trace.

## Modifiers

Wiki: [Modifiers](Tool-Modifiers)

- Extrude and Texture map: while a tool is open on the mesh and one of these previews is waiting, the mesh no longer shows the pending wave over the last result.
- Every modifier card's title keeps its operation's hue in a shade that is readable on the card in both light and dark mode; the orange of Loft, Extrude, Hair and Lattice and the green, amber, teal and cyan titles were too faint on a light card.
- Selecting a large body with a Difference under an Extrude no longer freezes the app while the Extrude card counts its walls, and no longer keeps up to 900 MB of memory for the rest of the session.
- Booleans and Extrudes on large meshes spend less time preparing the mesh before each result.
- Documents with hair that were saved before this version show a notice when you open them. Their hair is regenerated on the new weld, so printed strands can differ slightly from the last export; saving the document again clears the notice.

### Extrude

- The Path picker has one ✕, which removes the path.
- A new Shape section leans the walls. Draft sets an angle from the direction of travel, up to 60° either way: a positive angle narrows a raise as it rises and a cut as it goes in, and a negative one widens it. End scale sizes the top as a share of the base instead. The section starts closed and says what it holds ("Draft 10°", "End scale 0.5").
- When a draft would fold the top over, or the surface curves away, a line under Depth says how far the Depth is held. A large draft on a small face stops short of the fold instead of turning the top inside out.
- Dragging Draft or End scale moves the walls as you drag, as Depth does, on a mesh of a million triangles.
- Bake keeps the leaning walls and writes the triangles the viewport draws.
- The Shape section takes a Twist, up to two turns either way, that turns the top about the direction of travel through the center of the face, and the walls follow it in a spiral. Along Normals has no single direction to turn about, so the Twist slider is off there and says why.
- A new Wall segments section places the rings that carry the turn. Auto adds as many as keep the walls within a Tolerance (0.05 mm to start) of the spiral; Count sets the number yourself. When the rings placed leave the walls farther than the Tolerance from the spiral, a line says by how much.
- Dragging Twist turns the walls as you drag on a mesh of a million triangles. The drag stays smooth while the ring count holds, and builds the result when a step needs another ring.
- Committing a model's measure (Inspector, Units) scales the Tolerance along with Depth and Inset.
- Bake keeps the twisted walls and writes the triangles the viewport draws.
- Wall segments gains Bands and Sides. Bands divides the walls into equal bands along the travel, at most one Pitch each, and Sides divides them at every corner sharper than an Angle. Each band and side is a wall group of its own, so a rim can print in stripes or one side of a boss can take another color.
- The Wall Groups section lists a row for every side and band of a face group, with its number, its name and a color that sets the walls' material for that segment alone; a segment without a color of its own draws in its group's walls. Bake writes each segment into the mesh with the material the view drew.
- With Bands on, the rings are placed so each band is a whole number of rings, and the card says when that raises a Count you set. Dragging Depth past a change in the number of bands redraws the walls once instead of sliding them.
- A Path in the Shape section sweeps the chosen faces along a Wire, so a handle grows from a face and bends as the Wire does. Pick the Wire with the usual target picker, in the scene or in the Object Browser. While a path is set, the Wire is the direction: Depth, Direction and End Limit give way to a Length along the Wire.
- Length runs along the Wire, to its end by default; a negative Length cuts a groove along it. Taper and Twist grow along the Wire's length.
- A sharp corner in the Wire (a pin with no tangent weight) takes a miter, so the handle turns without a gap. On a curved Wire the rings sit close enough that the walls stay within the Tolerance of the curve.
- Where the Wire turns tighter than the profile is wide, the sweep is built anyway and each fold is marked on the Wire in the print-warning color and listed on the card with where along the Wire it sits. A Wire that comes back through its own sweep is listed the same way once it holds still.
- The card says when the Wire is gone, has no length, starts into the surface, or starts far from the faces, in which case the profile still follows the Wire's shape from where it is.
- New wire from faces, in the Shape section, places two pins at the faces' center and one face-width above along their average normal, joins them with a Wire under the mesh, and picks it as the path. The Wire tool bends it from there. One undo covers all of it.
- Dragging a pin of the Wire redraws the sweep on a mesh of a million triangles.
- Bake keeps the swept walls and writes the triangles the viewport draws.
- With a tool open on a large mesh, a Depth change that takes longer than 0.3 s to build draws an outline of where the new extrusion will stand over the last result, and removes it when the new result is drawn. A change that finishes inside 0.3 s draws nothing.
- On a large mesh, a change of an Extrude's settings with a tool open now outlines the new extrusion whatever else is in the stack (a modifier after it, a second Extrude, a Twist or Draft change, a Depth through 0, a sweep along a Wire), and the change lands in seconds where it waited up to twenty.
- Dragging a pin of the Wire an Extrude follows, with the Extrude's card open, rebuilds the card's wall rows once the drag ends instead of at every step, so the drag holds 60 fps through the card.
- Dragging Depth on a mesh that a Combine boolean has cut no longer sends the whole mesh to the worker at every step or computes the boolean again, so the drag follows the slider on a million-triangle mesh.
- Changing an Extrude's Depth, Inset, Draft, Twist or path on a large mesh no longer freezes the page. The mesh you were looking at stays on screen and the new extrusion replaces it when it is ready, and the viewport keeps responding while it works (a 5-million-triangle mesh froze for about 4.5 seconds before).

### Hair

- A band or side of an Extrude's walls can be picked as a hair region; it grows once the Extrude is baked, as for any wall group.

### Skeletonize

- When a lattice setting takes longer than 300 ms to rebuild, a violet grid at the lattice's spacing appears over the body until the new lattice is drawn. Rebuilds that finish sooner show no grid.
- The Edges and Dual edges shell layouts no longer crash the page on a model of a million or more triangles, when the shell follows the model's polygons exactly (deviation 0).
- Building a skeleton on a mesh of a million triangles or more is faster. The default truss takes about 40% less processor time, and the Shell edges and dual layouts 10 to 20% less.

### Lattice

- The summary card keeps one height when a coarse preview is replaced by the full-detail result, so the sliders below it no longer jump.
- Building a lattice on a model of a million or more triangles no longer crashes the page, including on a second large model in the same session.

### Booleans

- A cut through a flat face keeps that face's own triangle edges, so the result can have a few more triangles there; its shape is unchanged.

## Bake

Wiki: [Bake an SDF to a Mesh](Workflow-Bake-an-SDF-to-a-Mesh)

- Bake, Split and Refine: Cancel on a body's spinner now stops Split and Apply, and drops the result of a pose bake, Harmonic projection or trace paint. A bake that cannot stop shows a spinner without a Cancel button.
- Selecting a Loft's Trace, or the body its first section lies on, lists the Loft in the Bake panel, and Bake turns it into a mesh.
- A Loft's chip can combine the baked Loft with the body its first section lies on, with the body its last section lies on, or with both, which merges two bodies into one solid. Consume Traces deletes its section Traces. All three start off, and Bake remembers the last choice.
- A Loft reached only through the body its last section lies on shows a notice naming it and saying what to select to bake it; the Loft stays live.
- The grid shown while the clay meshes follows the light and dark themes (violet-600 and violet-400).
- The decal preview shows the same violet outline past 300 ms, and its mesh no longer shows the pending wave or a row spinner while the preview computes. Bake's violet grid is unchanged.
- Baking a Combine of large meshes no longer shrinks the body, its selection box and its measurements to a point for a moment before the baked mesh appears. The body keeps its size and stays marked as updating until the baked mesh draws.
- Committing a mesh with Difference or Union tool bodies now shows the in-progress state on the mesh and on each tool body until the baked result is in, instead of leaving them unmarked for the seconds a large boolean takes.
- Combine, Bake and Export: a mesh boolean on a 5-million-triangle mesh completes, where it showed the mesh uncut or kept computing without end. A boolean too large for the boolean engine's memory stops with a message that says so, and the booleans after it work.
- The wireframe Bake shows on its preview is no longer saved as your wireframe setting, so closing the tab while Bake is open no longer starts the next visit with the wireframe on.
- Baking a texture map (color, displacement, density or normal) or a decal with Refine mesh on a mesh of a million triangles no longer freezes the view; the mesh shows that it is updating while the work runs.
- The preview of a clay with several shapes or face groups, and the Bake of a clay with several shapes or a color map, no longer hold the page while the colors and face groups are worked out at a million triangles; the work runs in the background with the mesh.
- A mesh with an Extrude now bakes to the same triangles the viewport shows. Bake used to rebuild the extrusion through a separate boolean, which gave a mesh with more triangles (47% more on a sphere's raised cap) and, where the extrusion's edge met a hole, could nick the hole's wall. Extrudes whose new faces cross the surface still bake through the boolean.

### Refine

- When a preview takes longer than 300 ms (a large mesh), violet lines appear on the last preview at the density the new setting will give, and "Computing preview…" appears with them. Previews that finish sooner show neither.

## Split and Combine

Wiki: [Split and Cut a Model](Workflow-Split-and-Cut-a-Model)

- Split's plane preview and Split itself no longer hold the view while they find the cut loops on a mesh of 200,000 triangles or more: dragging the plane on a 1.2M-triangle mesh stays smooth, and a 5M-triangle split no longer freezes for over a second before it starts.

### Split

- The cut loops follow the plane while you drag it through a mesh of millions of triangles.
- The cut faces of a split are flat caps that both pieces share, so the two pieces fit back together exactly and each is a closed, watertight body.
- A hollow body cut above its cavity gives two pieces, with the cavity kept inside the lower one, where it used to give an extra third piece turned inside out.
- A cut that cannot be closed cleanly, such as a plane that skims a surface closer than the finest detail the model can hold, is refused with the place and the reason, and the body stays as it was.
- Surface points within 5 µm of the cutting plane are moved onto it, so a plane that only grazes a body makes a flat cut or no cut instead of a sliver.
- A Cap strategy menu chooses how each cut face is closed: Flat, Liepa, Star, RBF or Smooth. Hovering a strategy says what it does and when it falls back to Flat; the choice is remembered.
- A Clearance slider sets how close to the plane a surface point may lie before the cut treats it as on the plane, from two lattice steps up to 0.1 mm, 5 µm to start. Under the slider, a line says how many vertices the cut moved onto the plane and the largest move.
- The cut list has one row per part of the cut that separates the body, each with its loop count. A cut that leaves the body in one piece (one cut of a ring that crosses the plane several times) shows why under its row and keeps Apply off until another cut of the ring is selected.
- Selected cut loops draw as construction wires, the loop under the pointer (and its row) in the selected-construction color, and loops left out dashed in gray.
- Undoing a split and redoing it from history reproduces the cap strategy and clearance it used.
- Split can cut along a closed Trace as well as along a plane. Choose Trace at the top of the Split panel, pick a closed Trace on the body (a Trace that is part of a closed loop of Traces picks the whole loop), and Apply cuts the body into two pieces along it. The Trace is drawn in the construction blue, and a translucent preview of the cut face appears over the body while you work.
- The face that closes a Trace cut is a smooth surface stretched across the loop, so a Trace that does not lie in one plane gets a curved cut face. Where a face would touch the body's surface, it is pushed into the body until it clears. The two pieces share the same cut face, so they fit back together.
- When a Trace cut cannot be made, the Trace's row in the panel says why, and Apply stays off. The reasons are a Trace that does not close along the surface, a Trace that does not divide the body, a cut face that folds or will not settle, a body too thin for the face to fit, and cut faces of two Traces that cross each other. Traces that cross or touch each other cannot be cut together yet: Split says so and cuts nothing.
- A plane that passes exactly through a mesh's vertices now finds its cut. Before, the plane the tool places when it opens, through the center of a UV sphere, said "No cuts on this plane" until it was moved off the vertices.
- Accept closes the tool at once. The body waves in the scene and spins in the Object Browser while the pieces compute, and Cancel on the spinner puts the body back. You can keep working in the scene meanwhile.
- The preview stays smooth on large meshes. Each half draws once, solid, in blue (Above) and orange (Below), the wireframe is drawn once, and the faint copy of the original that used to show under the preview is gone.
- The preview tints the model itself instead of drawing it again for each side, so dragging the plane stays smooth on large meshes. The Above (blue) and Below (orange) colors follow the light and dark themes, and the wireframe is the model's own.
- Dragging the plane through a mesh of a million triangles or more no longer stalls the view at each new position; the cut loops follow the plane as it moves.

### Combine

- While you drag a tool body of a mesh union, intersect or difference with the Move tool, the curves where the two bodies meet are drawn through both bodies and follow the drag. On large meshes the result itself is computed once the drag stops, instead of during it.
- When two bodies touch over too much of their surfaces to trace within a frame (noisy scans crossing at a shallow angle, surfaces that nearly coincide, flat faces that lie on each other), the curves stop for the rest of that drag, and Move and Combine say so, with the count of pairs to test past which they stop. The result still appears once the tool stops moving.
- When you stop dragging a tool body of a large mesh's union, intersect or difference with the Move tool, the translucent curves where the bodies meet turn opaque and close, including where an edge of one body lies exactly across an edge of the other. They stay until the new result appears.
- When the result of a large Combine takes longer than 0.3 seconds, each tool body's outline shows through the target in the preview color until the result appears. This covers opening Combine and changing its operation.

## Trace

Wiki: [Construction and Rigging](Tool-Construction-and-Rigging#trace)

- Cache and Loop Type are translated, and Cache reads "Cut into the mesh", "Follows the nearest edges" or "Not computed".
- A wire used as a trace source now projects as a closed trace when the wire is closed. The trace follows the wire to a tolerance you set (0.05 mm at first), where it used to take 64 points however long or tight the wire was.
- A wire's trace can be projected two ways, Closest point or Along up. Along up casts from each point of the wire in the direction of its pins' up axes, so a loop above a concave face lands directly below it and keeps its outline. The Trace panel names a wire that has no surface along its up, and warns when the projected trace crosses itself.
- The Trace panel has a Wire picker, so a wire can be chosen from a list or by clicking it in the scene.
- A trace's points can be corners. A trace made from a face group's outline gets a corner wherever the outline meets a different neighboring group, so the outline of a box's face has the box's four corners.
- Hold Alt (Option on a Mac) and click a vertex to make it a corner or clear it, or turn on the Corners switch and click. Corners show as orange diamonds. Each trace in the panel shows its corner count, and Tag corners places them wherever the trace crosses from one face group to another.
- The Cache row is named Split cache, so its "Not computed" for a Trace that a Loft follows along the mesh's edges no longer contradicts the Loft card.
- Trace and Scribe: the highlight Trace puts on the face group under the pointer, and Scribe's eigenvector colors, no longer let the model show through in small patches where a surface faces the camera head on, on large models.
- A closed Trace whose first point sits exactly on a mesh edge or vertex now closes when it is split, where before it stayed open and Split refused it.
- The Info panel for a Trace scrolls, so a Loft modifier under its details stays reachable.
- Split mesh keeps each vertex's own shading. Before, a few percent of the vertices on a split mesh took another vertex's normal, so the surface showed dark or reversed spots, and this fix removes them.
- Split mesh gives the same result however the mesh was loaded. Before, a trace drawn through vertices could split a mesh differently when the same model was stored in another order, so reloading a scene changed which triangles each piece took along the trace.
- Split mesh, Split all traces, Repaint edge and the Create Trace split run in the background. The view keeps drawing, the body shows it is busy, and Cancel stops the split with the mesh as it was. A 1 million triangle mesh used to stop the view for about eleven seconds, and a 5 million triangle split gave up after thirty.
- A split that fails says which mesh and why, and leaves the mesh unchanged.
- Project Traces (Harmonic) runs its embedding and split on the trace worker, with Cancel.
- Split mesh on large meshes finishes faster and uses less memory; a 1.6 million triangle mesh that ran out of memory now splits.
- Split mesh on large meshes takes a fraction of the time and memory. The split changes only the triangles along the trace and leaves the rest of the mesh as it was, so a mesh of millions of triangles splits in about the time it takes to draw the result. Repaint edge on a split trace likewise works on the triangles beside the trace.
- Split mesh no longer repairs holes, gaps and other defects far from the trace; Repair handles those. A defect the cut itself reaches is still repaired, and if a repair needs more room than the split allows, Split mesh stops with the reason and leaves the mesh unchanged.

## Pin and Wire

Wiki: [Construction and Rigging](Tool-Construction-and-Rigging)

- A Closed switch in the Info panel returns the wire from its last pin to its first, so three pins or more bound a loop. The first pin gets an incoming tangent and the last an outgoing one. An Array placed along a closed wire follows it all the way round. A scene saved before this reads every wire as open.

## Move

Wiki: [Construction and Rigging](Tool-Construction-and-Rigging#move)

- When moving a tool body under a large mesh boolean takes longer than 0.3 seconds to cut, the tool body's outline shows at its new place until the new cut appears.

## Repair

Wiki: [Repair a Scan for Printing](Workflow-Repair-a-Scan-for-Printing)

- Opening Repair on a mesh no longer marks the mesh with a spinner while it is analyzed. The panel's "Analyzing" line still shows.
- Accept, Repair now and the Repair script operations repair a large mesh in the background. The view keeps drawing, the body shows it is busy, and Cancel stops the repair with the mesh as it was. At 5 million triangles the view used to stop for about six seconds.
- The panel finds a mesh's holes, cracks and wrongly wound faces in the background, so opening Repair on a large mesh no longer stops the view.
- A repair that fails says which mesh and why, and leaves the mesh unchanged.
- Repair, Trace and Crack: When Accept runs work in the background, the tool stays open and its panel shows a progress line with Cancel. Cancel puts the scene back as it was before Accept, and the tool stays open so you can change your choices and accept again.
- Applying Repair's fixes to a large mesh takes less time, because the weld and the edge pairing it needs now run in the compiled mesh module. The repaired mesh is the same.

## Crack and Scribe

Wiki: [Surface and Refine](Tool-Surface-and-Refine)

- Scribe, Part and Crack: The "Analyzing" line and progress bar appear only when opening the mesh takes longer than 300 ms, so a small mesh no longer flashes them.
- Closing Scribe gives back the copy of the mesh it analyzed, which it kept until it was next opened.
- Crack groups a large mesh's faces sooner: on a 400,000-triangle mesh its group pass takes about 0.3 s, down from 0.4 s.

### Scribe

- Closing the tool while it analyzes a mesh stops the analysis. Before, the analysis of a large mesh ran on for minutes after the tool closed, kept a processor core busy, and redrew the view every few seconds.

### Crack

- Opening the tool on a mesh of a million triangles no longer freezes the view while Crack prepares the mesh for the loop search.
- Refine and Subdivide on a mesh of a million triangles no longer freeze the view, with the same sign on the mesh and a Cancel. A failure says why and leaves the mesh as it was.
- Opening Crack on a large mesh no longer freezes the view for half a second while its groups are colored.

## Rig and Pose

Wiki: [Rig and Pose a Character](Workflow-Rig-and-Pose-a-Character)

### Pose

- Each frame of dragging a joint on a model of about a million triangles takes about 30% less time.

## Paint

Wiki: [Paint Decals and Texture Maps](Workflow-Paint-Decals-and-Texture-Maps)

- When a stroke over a mesh with a boolean takes longer than 300 ms to update the ghost, a violet rim marks the ghost where the stroke painted, until the ghost is redrawn.
- The brush's footprint, the stroke as you paint it, and the region a fill click would paint cover the whole area again. On a dense mesh seen face on, the body showed through them in irregular patches.
- On a large mesh, the wireframe no longer fades back in after each stroke; it stays as drawn while the stroke's colors update.
- A large brush stroke on a mesh of millions of triangles keeps up with the pointer and lets go without a stutter, since each dab of the brush finds its faces about three times faster.
- On a mesh with a boolean, the faint preview of the result shows a stroke's colors without computing the boolean again, so it comes back almost at once when Shift is let go (at 1.2M triangles, in a fraction of a second where it took about 2 s).
- Opening Paint on a large mesh that Repair or a trace split just edited no longer waits to prepare its brush surface; the surface is ready when the edit lands.
- Texture maps, Crack, image drops and Bake: on a mesh of a million triangles, a change no longer waits for the mesh to be copied to a worker and its picking structure copied back, so the scene settles sooner after each edit (hovering and clicking the mesh work as before).

### Decals

- Starting a new preview before the last one finishes keeps the spinner on until the newest preview ends.
- When a wrap, spherical or cylindrical decal takes longer than 300 ms to solve, a violet outline shows where the decal will land until the result is drawn. The body no longer shows the pending wave or a spinner in the Object Browser while the decal is solving, and there is no Cancel for it.
- Switching a wrap decal between Wrap and Spherical shows the new layout at once, instead of laying the mesh out again (about a minute at a million triangles). Cylindrical reuses the same layout on open meshes and wherever the image's center misses the mesh, and elsewhere lays out in a little over half the time it took.
- Moving, turning or scaling a wrap decal's image realigns the decal at once instead of laying the mesh out again.
- The outline of where a wrap decal will land, shown while its layout computes, now stays long enough to read and fades out, instead of vanishing the moment a short wait ended.

### Texture maps

- With a tool open on a large mesh, a change of a displacement map's scale that takes longer than 0.3 s to apply draws lines showing how far the surface will move over the last result, and removes them when the new result is drawn. A change that finishes inside 0.3 s draws nothing.
- Dropping an image on a mesh no longer freezes the view while the mesh is recolored, and changing a displacement map's settings freezes it for less time. The color preview shows the image's colors as the mesh has them once the map is applied, where it showed them lighter before.
- Changing a Texture map's image, strength, bias or density on a large mesh no longer freezes the page. The mesh you were looking at stays on screen and the new result replaces it when it is ready, and the viewport keeps responding while it works (a 5-million-triangle mesh froze for about 1.5 seconds before).

## Objects and the interface

- Modifiers, Images and the scene view: the Add modifier button, an image whose file is missing, and a body whose X-ray is switched on or off no longer risk an error screen when the selection or the body changes.
- A panel whose code cannot load (storage the browser cleared, for example) says what did not load, with a Reload button that saves the open document first, instead of replacing the editor with an error page.
- If part of the app can't load (for example after an update while the page was open and offline), an error names the feature and asks you to reload the page while online.

### About

- Open Source Software lists ExifReader, meshoptimizer, glTF-Transform, three-mesh-bvh, troika-three-text and ufbx, and its Comlink and OpenCascade links point to their own projects.
- Kelcite.app/faq opens the FAQ and kelcite.app/changelog opens About scrolled to the changelog, at full height. /faq used to land on the document list without opening anything.
- The FAQ, the changelog and each tool's tutorial are also plain web pages under kelcite.app/docs/, in English, each with a Markdown copy, so search engines and language models can read them without running the app. kelcite.app/llms.txt, sitemap.xml and robots.txt list them.
- A tab left open while a new version installs keeps working on the version it loaded. Its panels and tools still load, it no longer reloads by itself, and the update banner offers the new version.

### Preferences

- Antialiasing in the new Display group sets whether edges are smoothed, with Auto (the default), On and Off. Auto turns it off on displays at a pixel ratio of 2 or more, such as Retina screens and phones, where edges already look smooth. Off saves about 80 to 100 MB of graphics memory on a desktop canvas at a pixel ratio of 2 and about 21 MB on a phone. A change redraws the view at once, and the scene, selection and camera stay as they were.

### Camera

- The camera now keeps the view you left when the scene view is redrawn, such as after resuming a stopped scene.
- Orbiting about the point under the cursor, and the camera's re-pivot after a pan, now land on a Loft's body. Before, the view turned about whatever lay behind the body.
- The ground, grid and axis lines show at full strength around a body. The contact shadow darkens the ground only under the body, where before it also washed the ground, grid and axis lines in the theme's background color.

### Objects

- A body's spinner while a bake, split or apply runs is now a button you can reach with the Tab key. Enter cancels the operations that can stop, and the spinner names the operation and the body. The body's name stays visible beside it instead of being replaced by the operation's label.
- A body that is being rebuilt or drawn for the first time shows a spinner in its row once the work has taken a third of a second, so a quick undo shows nothing. A spinner that can cancel its operation is a button with a name, and the body's name stays in the row.
- Select closed loop on a Trace keeps working after you rename the Trace or its neighbors. Loops are now found from where Traces meet, where the names used to decide.

### Accessibility

- A screen reader announces the operation and the body when the wait begins ("Baking Sphere…"), once however many undos pile up, and says when the work fails.

### Info panel

- A node a card names (a Loft's sections and matches, a Boolean's tool bodies, a Profile's outline) is one gray chip that lights the node in the viewport when hovered or focused with the keyboard.
- What a build found reads as one line with an icon: a note, a warning (the result holds with something given up) or an error (nothing was built), in colors that stay legible in light and dark mode.
- Every remove, reorder and pick button shows its name in a tooltip and a focus ring.
- A progress bar (a lattice or hair rebuild, a Combine, Crack, Trace or Repair that works on a large mesh, the Scribe eigenvectors) floats over the top of the panel instead of opening a row in it. The controls under it no longer shift when the bar appears, so a slider you are dragging on a touch screen stays under your finger, and the bar stays visible however far the panel is scrolled.

### Pickers

- The empty placeholder of the Extrude Path, Array and Hair pickers reads "Select a Wire" instead of "Select a a Wire", and the Array surface picker reads "Select a surface" instead of "Select a None".

### Languages

- Trace and Traces now read as singular and plural of one word in Czech, Dutch, French, Polish, Portuguese and Spanish, and the Rig tool's "Faces" label reads as a facing direction in Czech, Polish and Russian instead of "forward".

### Touch devices

- The app menu on iPad and phones, in a scene and on the start screen, has the Performance overlay item, as the desktop menu does.

### Panels

- A panel group popped out to its own window no longer loses its panels when Kelcite opens again. The group comes back as a floating panel group in the main window, and Pop out reopens it as a window with a click.
- The Float, Dock and Pop out buttons on a panel group's header are translated and carry labels for screen readers.

### Asset Library

- Saving a mesh from the scene and loading it back no longer builds a text copy of every vertex, so a million-triangle mesh saves and loads in a fraction of the time and takes about a quarter of the space. Meshes you saved earlier still load, and are stored in the new form the first time you use them.

### Undo

- Export History files from earlier versions still import and replay.

### Face groups

- Right-clicking a mesh of millions of triangles the moment after an edit that rewrites its triangles opens the face-group menu about 0.5 s sooner, and Paint's brush and Trace's face click wait, with a busy cursor, for the edited mesh to draw instead of picking faces from the mesh it replaced.

### Images

- Blurring a large image for a displacement map or a mask runs on the graphics card where the browser offers WebGPU, so a 4096 by 4096 image's blur takes a fraction of a second, down from about a second. Other devices blur on the processor as before.

## Files and the tour

Wiki: [Getting Started](Workflow-Getting-Started#the-demo-tour)

- Scenes you save now record a newer file format. An older Kelcite that opens one says it was saved by a newer version, instead of quietly leaving out Profile clay, Lofts, closed Wires and Extrude's taper, twist and path.
- Load Scene is a grid button at the top, left of the scene's name, and no longer in the File menu. It lists your scenes to load.
- Demo is the last entry of the File menu and no longer a button in the toolbar. Searching for Demo, with the tool search, still opens it.
- Imported STL files open as shared-vertex meshes, so Repair, Split and edits on a large STL start sooner after the import.
- Import, Combine, Bake, Repair and Commit units: vertex positions of a mesh you create or edit with them are rounded to a fixed grid of 15 nanometers (millimeter scenes; the grid is 2^-16 of a unit, so 15 micrometers in a scene measured in meters). Two points that were the same point before now stay the same point in every tool, and exports from those meshes can differ from earlier exports in the sixth decimal place. Meshes in saved documents change only when you edit them.

### The tour

- A new Clay profile stop shows a star outline and the twisted, rounded knob extruded from it.
- The Loft combined exhibit shows a boss joined to its plate.
- The Loft matching pins exhibit shows a star lofted to a square with one pinned corner.
- The Extrude taper exhibit shows a drafted boss beside an end-scaled one.
- The Extrude twist exhibit shows a grip twisted a quarter turn each way.
- A new Extrude bands stop shows a rim in three bands and a boss with two colored sides.
- The Extrude path exhibit shows a handle grown round a corner beside one grown along a curve.
- The Loft exhibit shows a boss lofted from a plate's traced top square to a circle.
- The next button now sits right after the exhibit title, so on a phone it no longer touches the close button; the title shortens with an ellipsis when it is long. On a touch screen the previous, next and close buttons are each 44 px square.

### Import

- A file of many small parts, such as 500 boxes in a file under 1 MB, shows its progress scrim as soon as the file is read, so the page no longer freezes for a quarter second with nothing on screen.
- A large model no longer freezes the view for a second or more right after it arrives. The step that lets you undo the import used to compress the whole mesh before the view could draw again; it now keeps the mesh as it is.
- A large 3MF file takes less memory to open.
- A 3MF of 1,000,000 triangles opens in about 3.0 s, down from 3.3 s; one of 400,000 triangles, 2.6 s, down from 2.8 s.

### Export

- Saving a large mesh as STL or 3MF no longer freezes the page while the file is built; the page stays responsive through the export of a 1.6-million-triangle model.

## Rendering and performance

- Large meshes use much less memory: checking whether a mesh is watertight and measuring its volume and area now takes about 70 MB for a 1.2-million-triangle body, where it held 500 MB until the page was closed, and it finishes about five times sooner. The results are the same as before.
- A body that is updating after Bake, Refine's Apply, Repair, Trace or Crack stays marked until its new mesh draws, instead of losing the mark while its old drawing still shows.
- A large mesh whose rotation or scale is applied to its points keeps its place and size while its new drawing builds.
- Memory goes back after heavy work: about a minute after a large Combine or Paint finishes, the memory its background work grew to is returned, instead of staying held until the tab closes.
- Deleting a body with a Combine or an Extrude gives back the memory its result used, instead of keeping it until the document is closed.
- Graphics memory goes back once work ends: after a large Bake, Combine or clay session, the memory its previews and checks used is returned to the system instead of staying held until the tab closes.
- Deleting a large body, or starting a new scene, gives back the memory its drawing and its welds held, including a split body's pieces, instead of keeping it until other bodies were drawn.
- After 8 s, the panel offers Show the scene now, which shows the scene while the rest finishes. Objects whose shaders are still compiling stay hidden, with a spinner in the object browser, and appear as each is ready; the notification log names the steps that were skipped.
- A shader that has compiled for a minute without finishing is reported in the notification log, with links to the objects it keeps hidden.
- Bodies cast contact shadows on the ground again: with shadows and the grid on, the ground darkens under each body and follows it when it moves.
- The slow-scene warning no longer appears for the moments after a large operation such as committing a Bake to 1.2M triangles, when the page is busy drawing and compiling what the operation made.
- A slow-scene warning that nobody has answered closes by itself once the scene has held 20 fps for as long as it took to appear, even if you are not touching it. Stop drawing, the warning's other buttons and dismissing it work as before, and a scene you stopped drawing stays stopped.
- Kelcite opens faster: the code for text, notes and the FAQ, image details, model import and export, SVG decals and booleans now loads the first time you use it, not when the app starts. A body with booleans shows a soft fog for a moment while the boolean engine loads, and an SVG decal shows its pixels until its shapes are read.
- Trace snapping, the material menu's region count and Repair now's check read large meshes in the background; opening Part on a 1M-triangle mesh holds the view for about 0.2 s instead of 0.3 s.
- Opening a document that is already open in another tab no longer asks "Open without drawing the scene?". Kelcite asks only when an earlier session of that document really ended without closing.

### Large meshes

- While an undo, redo or edit rebuilds a large mesh, the body that stays on screen is shaded toward the neutral pending gray (light and dark themes) in a rising wave, so it no longer looks finished when it is out of date. The wave appears after a third of a second, so quick rebuilds show nothing, and it stays at least a quarter of a second, so a run of undos reads as one steady wave. With reduced motion on, the wave holds still as a band.
- Removed or replaced bodies stop holding geometry memory past the cache's limit, so large scenes use less memory after repeated edits.

### Viewport

- Wireframe and X-ray on together show only the wires. The faces draw nothing, and every edge draws whether it faces you or not, so the edges at the back of a body show through the ones at the front. The wire keeps its color setting in the light and dark themes. Turning either one off returns to X-ray alone or wireframe alone, which draw as before.
- X-ray with the wireframe on no longer shows dark wedges and hatched fans across large bodies.
- The camera pivot diamond no longer casts a contact shadow on the ground while the view turns.
- Zooming far in or far out no longer clips or flickers. The ground, the grid, thin layers, decals and bodies stay solid from 0.1 mm away to a building's scale; the view used to cut everything closer than half a millimeter and tear a 0.2 mm layer apart at 10 m.
- Zooming in and out no longer leaves graphics memory behind with shadows on; before, each zoom past half a meter from the origin held on to about 32 MB until the page reloaded, and the app slowed as it piled up.
- While nothing changes, the app draws no frames and uses no GPU. A document left open, a tool left open, or the performance overlay left open all stay at zero until you move, click, type, or something in the document changes.

### Wireframe

- Wires-only view (wireframe with X-ray) draws the mesh once instead of twice, which halves the triangles it draws.
- Every edge now draws cleanly, as a continuous line where the old one broke into dashes at a close view, and the wireframe costs far less to draw, so orbiting a large wireframe mesh is smoother.
- Preferences has a Wireframe line weight slider beside the wireframe colors, to make the lines lighter or heavier.
- Bake's preview and Refine's preview draw their wireframe from a list of edges made in the background with the mesh, so a million-triangle preview no longer freezes the page for a moment at each change of a setting while the wireframe is on. The wireframe shows the same edges as before.
