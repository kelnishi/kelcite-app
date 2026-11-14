# Changelog 0.17.390

Released 2026-11-13.

A Loft can now follow a Wire (a **spine**) and be held to **guide rails**, so a handle can bend, twist and swell between its ends. Clay gains a **Sweep** that carries a section along a Wire, and Split can join its two cut faces with a Loft. Baked clay keeps the names of its face groups, and Move, Jig and the tour change in smaller ways, listed by system below.

## Clay

Wiki: [Sculpt with Clay](Workflow-Sculpt-with-Clay)

### Sweep

- A **Sweep** carries an SVG, text, closed Trace or closed Wire along a Wire as clay, so it bends with the Wire and stays editable until baked. Its card in the Info panel picks the **Section** and the **Path** and sets **Taper** and **Twist** over its length. The demo tour's **Clay sweep** stop shows one.
- A Sweep follows its section and its Wire as they change. Where the Wire bends tighter than the section is wide, it keeps its last shape, and its card says where the Wire folds.
- A Sweep follows the Wire's own curve, so it shades smooth along a bend instead of showing a band at every ring. A sharp corner, where a pin's tangent weight is 0, stays an exact miter.
- A Sweep that fills the screen no longer slows every frame: a still view draws it once, and while the camera moves or a pin or slider is dragged it draws at a quarter of the pixels, sharpening when you stop.
- A scene with a clay Sweep saves as file format version 5, so an older Kelcite opens it as far as it can and says it came from a newer one.

### Outline and Profile

- An Outline or a Sweep of an SVG or text follows the drawing's own curves, so a circle is round instead of showing a flat face every two SVG units. Overlapping shapes, strokes and a Corner radius still read the outline as before.
- Dragging **Corner radius** on an Outline keeps the interface responsive. The body follows the slider and shows the last value when you let go, where it could freeze the page for seconds before.
- A Corner radius that rounds away the whole Outline says so under the slider, and the body stays empty until the radius is smaller, where it could keep recomputing in the background.
- A polygon saved without a Star or Polygon setting draws as a star in the live preview, as it already did in the baked mesh and in the Info panel.

### Creating clay

- **Create** right after opening the Clay tool no longer freezes the page while the shape's shaders finish preparing, and the new clay appears sooner. The freeze lasted about 5 seconds in Chrome on Windows.

## Loft

Wiki: [Loft Between Shapes](Workflow-Loft-Between-Shapes)

### Spine

- A Wire can be a Loft's **spine**. The sections sit along it where it passes closest to their centers, and the walls between them follow its bends. Pick the Wire in the Loft card's **Spine** row.
- The spine's pins set its twist. Each pin's **North** fixes which way the outline faces at that pin, and between pins the outline turns evenly from one North to the next, so editing the Wire never flips the body or its ends. Turn a pin about its up to twist the Loft there; the card's **Twist** adds to that. **Upright**, in the Spine row, keeps the outline facing the way it does at the start instead.
- **Add a guiderail wire**, in a Loft body's right-click menu and its row's menu in Objects, draws the spine for you: a Wire through a new pin at the center of each section, aimed the way the Loft leaves it and facing the way the section does, so the Loft keeps its pairing. Each pin becomes its section's take-off Pin.
- **Link with Wire**, under the card's Spine picker while a Loft has no spine, draws a Wire through each section's take-off Pin, or the pin on its face (a Split cap's), adding a pin where a section has none, and makes it the spine.
- A pin inserted on a Loft's spine Wire (**Insert point on the Wire** on the Wire card) becomes a section of the Loft at that point. The Loft's shape does not change until you move or turn the pin: moving it bends the middle, turning it twists it. Removing the point removes the section.
- Along a straight spine, Ruled walls no longer run as long thin triangles from one section to the next. Rings sit about as far apart as the points around the section, as they do without a spine.

### Section twist

- Along a spine, each section's row has a **Section twist** slider in place of Start. It turns the section in quarter turns, up to ten turns either way, on top of the turn its pin's North gives, so a section can spin past 180°. Turn the pin for the fine angle and step the row for whole quarter turns.
- Spinning a section with the Move tool past 180° no longer flips the walls. Each time a pin's turn passes the half turn, the section's Section twist takes a whole turn, so the walls keep turning the way the drag goes. Undo takes back the move and the twist together.

### Guide rails

- Once a Loft has a spine, each further **Add a guiderail wire** draws a guide rail: a Wire through a new pin on an outline vertex of each section, and the outline follows it between the sections. Move or aim the rail's pins and the walls turn and swell to keep that vertex on the rail; with several rails, each one's vertex stays on its own. From a Loft body's right-click menu the rail starts at the outline vertex nearest where you clicked; from the card or Objects it starts across the widest gap between the rails already there.
- The card lists the guide rails under the Spine row. Hovering a rail lights its Wire, the **×** removes it from the Loft (the Wire stays in the scene), and **Add rail** draws another. A rail the Loft cannot follow says why on its row, such as a rail that runs back along the Loft, two rails that cross, or pins in another order than the sections, and the Loft builds without it.
- A guide rail's pin can go anywhere. At each section the rail follows from the outline point nearest its pin, so dragging a pin round the outline moves where the rail starts; it changes points only once the pin is clearly nearer the new one.

### Ends and take-off

- A Loft's last section now treats its take-off Pin as facing back, as a Wire treats its last pin, so the Loft arrives into that face instead of leaving it. Two pins placed on either side of a split now end a Loft into both cut faces alike.
- A Loft end on a Trace of another part, such as the other half of a split, ends on that part's own faces instead of capping flat with "This Trace is not on a mesh".
- Moving the body under a Loft's far end, such as the other half of a split with **Loft split bodies**, keeps the Loft's end on that body in every frame of the drag. On a large body the Loft trailed it by a frame or more.
- The fill of an SVG or text that ends a Loft draws cleanly over the Loft's flat end cap, without streaks of the cap showing through.
- A take-off pin inside the Loft's body or on a mesh is picked by clicking it in the scene, where it draws on top.

### Walls and card

- A Loft's walls are cut into rings along their length, so long straight walls are made of short, even triangles and Wireframe shows a grid instead of streaks running the length of the body.
- **Zebra** stripes a Loft body, and Smooth shading off draws it flat, as for any other body.
- The card shows the body's **Material** color at the top, under the line naming where the body sits, and no longer shows a Live body choice whose Clay option was not available.
- **Commit** on a model read in another unit carries its Lofts too, so matching pins and tagged corners stay on their sections and the Loft's chord tolerance follows the model's size.

### Editing speed

- Dragging Twist, a matching pin, a section or a take-off Pin redraws even a large Loft as you drag, without stalling the page; its shading is computed off the page.
- A Boolean that cuts with a Loft's body, Bake and export take the body as it stands when a drag ends.
- Moving an SVG section, or dragging a matching pin along a long outline, follows the pointer instead of pausing at each step.
- A Loft starts its update the moment a body it joins moves, so during a Move drag it keeps up with the dragged body more often.

### Build warnings

- A Loft that cannot build, or builds a broken body, puts a warning triangle on its own row, and on its Trace while the rows are collapsed. Hover it for the reasons the Loft card lists: sections that sit on each other (as the two faces a split leaves do), a section, pin or spine with a point that is not a number, walls that fold through themselves, a body that is open or not a clean solid, and a build that fails or does not answer.
- The warning goes away by itself once the Loft builds cleanly, for example when you move the split pieces apart.
- A Loft with no body yet, such as one right after a split whose two faces still touch, appears while you drag a part apart, as soon as the gap allows it, instead of when you let go. Its warning clears at the same moment.

### Names

- The two Traces that a face group's Loft and its destination pick make are named from their bodies, so the section picker and Objects tell them apart: bodies "Left hand" and "Right hand" give "Trace_Left" and "Trace_Right", and two Traces on one body, or bodies whose names give no clear pair, give "Trace_A" and "Trace_B".

## Modifiers

Wiki: [Modifiers](Tool-Modifiers)

- **Add Sweep**, in the search and in a Trace's or an SVG's **Add Modifier** menu, carries the selected section along the selected open Wire as a body of its own, from where the section is to the Wire's end. Text, SVG shapes and Traces all sweep. A Trace on a part keeps its weld, so a channel swept into a case combines into it as a Difference.

## Bake

Wiki: [Bake an SDF to a Mesh](Workflow-Bake-an-SDF-to-a-Mesh)

### Face group names

- A baked clay keeps the names of its face groups (Top cap, Side 1, Side 2 and so on, Bottom cap). Paint, Trace, the Inspector's Materials, the face group menu and the modifier cards show the names instead of numbers. The names show in the language Kelcite runs in, and a saved file keeps them.

### What bakes

- Baking a mesh's modifiers writes its booleans and extrudes into the mesh and keeps its decals, texture maps, masks and constraints, which were removed with them before.
- A selection whose only modifiers are constraints no longer opens Bake, which had nothing to bake there; the toolbar says "No bakeable operations on selection". A constraint beside a Boolean stays off the Boolean's card, and stays on the body after the bake.
- A Loft baked to a mesh keeps the visibility and X-Ray its row had.
- After Bake writes a boolean, the selection box measures the body alone; it no longer grows to include the hidden Cutter.
- **Dual Contouring** keeps sharp ridges thinner than a cell, such as a star's points, as straight creases where they run across the grid. A Sweep's twisted or curved ridges no longer come out sawtoothed, and the **Crease** option works on them.

## Split and Combine

Wiki: [Split and Cut a Model](Workflow-Split-and-Cut-a-Model)

### Loft split bodies

- Each selected cut in the Split panel has a **Loft split bodies** checkbox. Checked, the split puts a Trace on each of the two faces the cut leaves and joins them with a Loft, on the lower piece, whose sections take off from the pins Split places on those faces. The Loft leaves one face straight out and arrives straight into the other; it has no length until the pieces are moved apart. A cut whose face has a hole is not lofted, and an alert says so.
- Split names each cut's two cap Traces after their pieces, "Trace_A" on the piece above the plane and "Trace_B" on the piece below, instead of "Trace" for both.

### Names and display

- Each piece keeps the names of the face groups it came with, and the face a cut closes is named **Cut face**.
- The pieces list in Objects in the order of their names, the Above side's first, so after a horizontal split the upper piece is the upper row and the names read _A, _B down the list (_A0, _A1, _B0, _B1 when a side has several pieces).
- The plane, its outline, the cut loops, the gizmo and the Trace films no longer cast a shadow on the ground.

### Combine

- A Loft body used as a Combine tool is hidden on its row once the Combine is applied, as the tool's Trace is.

## Trace

Wiki: [Construction and Rigging](Tool-Construction-and-Rigging#trace)

- The boundary of a single named face group gets that group's name, such as "Top cap boundary".
- **Preferences → Keybindings** no longer lists "Cycle snap mode" under Trace. Snap is set on each trace's row, and Trace's key did nothing inside the tool.

## Pin and Wire

Wiki: [Construction and Rigging](Tool-Construction-and-Rigging)

- Only a left click on a pin adds it to a wire. A right-drag that starts on a pin orbits the view, and a middle-drag pans it.

## Jig

Wiki: [Construction and Rigging](Tool-Construction-and-Rigging#jig)

### Anchor Orient

- The **Anchor Orient** item works. It places a pin by a click on a surface or an existing pin, as Pin Orient does, and Preview and Apply add what its form turns on: **Stick to Surface** (a plane, a mesh or a clay body, with an offset along the surface normal and **Align up to normal**), a **Position Lock** and an **Orient Lock**. Before, the item made nothing.
- Its surface, Position Lock and Orient Lock targets are picked with the crosshair or the list, as other references are, and the offset is a slider in the display unit. The item names what is missing before Apply: no option turned on, no surface chosen, or Align up to normal beside an Orient Lock.

### Constraints

- A pin's position constraint (Jig's Position Lock and Pin Position) and its surface snap move the pin's body to the target once, carrying the pin. The body used to go twice as far, and the pin was left off it at the target.
- Pin Distance moves a pin's body once, carrying its pins, and AABB Align moves a body's pins with it. Before, Pin Distance moved the body too far and left the pin off it, and AABB Align moved the body's pins twice as far as the body.

### Preview and items

- During Preview, the pins of Pin Orient, Pin Position and Pin Distance items follow the solve, turning with the pin where an Orient Lock turns it.
- Deleting an item during Preview takes its pins and constraints out of the scene at once and solves again for the items that remain.
- **J** opens Jig with a pin on a mesh or clay body selected, such as the pin Apply just made, so a pin item can be added to it without selecting the body first.
- Item names show in the app's language.

## Move

Wiki: [Construction and Rigging](Tool-Construction-and-Rigging#move)

### Opening and selection

- Opening the tool leaves the selection where it is. It moved the selection for a moment to where Move was last used, and wrote two undo entries, so an undo straight after opening put the selection there.
- Selecting only something Move cannot move, such as a Trace or a Wire, closes the tool, and the toolbar shows Move unavailable. A selection that also holds a body keeps Move open on the body, and so does clearing the selection.

### Rigs

- A selected rig moves, turns and scales as one body: the figure, its skeleton and its pose go together, from the gizmo, typed Position, Rotation and Scale values and the quick edits. Selecting the rigged mesh moves its rig. Each edit undoes on its own.
- Dragging a rig moves it as smoothly as any other body. While it moves, the figure is not posed again and the rest of the scene is not rebuilt.
- **Drop**, **Align**, **Distribute** and **Stack** measure a rigged figure in the pose it stands in, not the shape it was bound in.

### Dragging

- Dragging a body with the gizmo no longer makes the rest of the scene rebuild at every step. Rigged characters, clay bodies with modifiers, hair walls and note flags update only when the drag moves something they depend on, so in the demo tour a drag of the Move exhibit's block keeps up with the pointer.
- Dragging any body no longer redraws the scene's modified bodies, wires, Lofts and skeletons that it does not touch.
- During a drag, a body and the Lofts that join it are drawn at the same step, so a Loft no longer trails the body it follows. A body with nothing depending on it is drawn a frame sooner than before.
- Typed Position, Rotation and Scale values now apply to a selected group, which they left where it was.

### Pins

- A pin's red, green and blue arrows turn with it while you rotate it, during the drag and after you let go, instead of waiting until Move closes. A clay body's split plane on that pin turns with it too.
- A pin you rotated keeps its new direction when you select something else with Move still open; it used to snap back to where it pointed before.

## Repair

Wiki: [Repair a Scan for Printing](Workflow-Repair-a-Scan-for-Printing)

- **Tab** does what the Repair All button does, so it now welds the detected cracks as well. A hole that already has a patch keeps it, with the strategy chosen for it, instead of being patched again with the current strategy.

## Crack and Scribe

Wiki: [Surface and Refine](Tool-Surface-and-Refine)

- **Space** on a focused row of Crack's crack or group list, Repair's hole list or Move's pivot list selects it, as **Enter** does, and the tool stays open. Space used to accept the tool as well.
- In Scribe, **Esc** while placing seeds ends placement and keeps Scribe open, with its seeds and clustering. A second **Esc** closes Scribe.
- Crack's bite rings no longer cast a shadow on the ground.

## Rig and Pose

Wiki: [Rig and Pose a Character](Workflow-Rig-and-Pose-a-Character)

- A rig scaled unevenly poses as it would unscaled. A dragged joint reaches the point it is dragged to, and the rotate rings turn the bone without skewing it.
- The Rig wizard's markers, lines and planes, and Pose's limit arcs, no longer cast a shadow on the ground.

## Objects and the interface

### Loft rows

- A Loft's body has a row of its own under its Trace, SVG or Wire, in italics beside a dashed icon, numbered when the node has more than one Loft. The node gains a disclosure arrow that collapses these rows. The row cannot be dragged, renamed or moved, and its menu offers **Bake to a mesh** and **Add a guiderail wire**.
- The eye and X-Ray buttons on a Loft's row show, hide and X-Ray the body on their own, and each click can be undone. Hiding the Trace, SVG or Wire, or **Toggle all construction visibility**, leaves the body in view; a body hidden from its row is not exported, as a hidden object is not. Lofts in scenes saved earlier show their bodies, solid.
- Clicking a Loft's row selects its Trace, SVG or Wire, as clicking the body in the view does, and hovering the row outlines the body in the view.

### Selection and pickers

- After moving through the Object Browser with the arrow keys, selecting a body elsewhere (a click in the view, an alert's link) scrolls the browser to that body's row, and the arrow keys and Shift-click move on from it.
- After moving a body that a Loft's section sits on, clicks in the scene select and the background deselects again. A click made while the Loft's body was rebuilding could leave every later click ignored, while band selection still worked.
- Choosing a target from a picker's list, or clearing it, while the picker waits for a click in the scene ends the wait, so the next scene click selects again. The Jig tool's anchor and wire pickers and its distance target do the same.
- With nothing chosen, a reference picker's placeholder lists the accepted kinds in your language, for example "Select SVG, Trace, or Wire" for a clay Outline's source.

### Info panel

- The Type menu and the Sides and Points fields of a clay body fit inside the panel instead of running past its right edge, down to a panel width of about 240 px.

## Files and the tour

Wiki: [Getting Started](Workflow-Getting-Started#the-demo-tour)

### The tour

- A link to the tour (kelcite.app/glhf/kelcite-demo.glb) opens it on any browser, including one that has never opened it, with no "was not found" error and no stop in the file browser.
- The tour is no longer a file in the file browser. It starts over each time it opens, and the app keeps a copy on your device for offline use, replaced when the app updates.
- The first edit to the tour says that its changes are not kept and offers **Save a copy**. The copy opens as a file of your own, named kelcite-demo-copy, then kelcite-demo-copy#1 and so on. The tour's name at the top of the window saves a copy too.
- A tour file kept from an earlier version is removed if it was never edited. One you edited or renamed stays, under an address of its own, without its undo history.
- The file browser's Demo button is gone; **Open the tour**, at the bottom right, opens the tour.
- Playing the tour turns **Labels** on if they are off, so its flags show. They stay on when the tour ends.
- The Clay profile stop shows its twisted clay knob beside the SVG star; the knob had stood out of view, in another part of the scene.

## Rendering and performance

- Note flags hide with the **Labels** toggle in the view controls, as measurements do.
- **Zebra**'s stripes on clay bodies run clean along their edges, where the normal's rounding speckled them.
- The text box and its preview, Bake's grid, and the origin, axis and plane highlights a reference picker shows no longer cast a shadow on the ground.
