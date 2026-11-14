# Changelog Feature Release 0.16.160

Released 2026-10-31.

The **Array** modifier repeats a body in a line, ring or grid, hair runs to **meshes as walls**, and the Timeline **authors clips**. Crack gains editable points, clay reads 16-bit height maps, and Lattice gains unit-cell patterns.

## Clay

Wiki: [Sculpt with Clay](Workflow-Sculpt-with-Clay)

- In the view, and in exports made without baking, clay keeps its full round shape, where one side of a sphere could come out slightly flattened at coarse detail.

### Displacement

- Clay reads a 16-bit PNG's full 16 bits, instead of the 8 bits the browser decodes. On the Moon's 20 km of relief, a height step shrinks from about 77 m to 0.3 m, so heights no longer terrace at high exaggeration.
- A 16-bit PNG with grayscale, brightness, contrast or palette processing on is read at 8 bits, since that processing works in 8 bits, and an alert says so. Turning the processing off keeps the 16 bits.

## Modifiers

Wiki: [Modifiers](Tool-Modifiers)

### Array

- A new modifier repeats a mesh, clay or a rig's posed figure in a line, a ring or a grid. Add it from a body's Add Modifier menu, or with Add Array on a rig's group, which repeats the posed figure.
- Pick a mesh, clay or rig to repeat, then drag the handles in the view to set the spacing, or a ring's radius and arc, and choose the first variation. \] and \[ add and remove copies. Done leaves an Array modifier, which the Modifiers section edits from then on.
- Opening it with a body and pins, a wire or a trace selected uses them as the array's locators, in the order you selected them. With two bodies selected, the first is repeated and the second is the surface it lies on.
- One selected pin locates the array, and two or more make a wire through them for a line or a spiral; a ring takes the first pin as its center and the second as its start. A selected wire locates a line, or a spiral when the layout is a ring, and a selected trace locates a line along it.
- An array stands on a locator, which is a pin, any node or a construction plane. The card has a Located by picker, a Place Pins button for an array that stands elsewhere, and an Orientation section with a Flip (which of the locator's axes the subject's up points along) and a Turn about the locator's up.
- Picking a layout in the Array tool makes the pins it needs. They stand beside the body, not under it, so moving the body leaves the array where it is; move the pins, or a group holding the body and its pins, to move the array.
- Dragging a pin onto the body it places is refused with a notice, and the Located by and Start pickers leave out the body's own parts.
- Pins, wires and traces you picked stay yours, and the array never moves or removes them. One that sits inside the body being repeated is left out, and the card says so.
- The pins and wires an array made are removed with it, and by Materialize Nodes, unless something else refers to them. Duplicate, Copy and Cut take an array's pins along with the body.
- A line steps along its pin's north, and a grid's rows along its east. The Array tool's Line makes two pins and a wire between them: moving the end pin stretches the line and keeps the count, and adding pins or tangents to the wire bends it.
- A line on a wire is spaced by arc length, by Count (spread over the whole wire) or by Pitch (one every so far, as many as fit).
- Orientation sets how instances face along a path. Follow turns them with the wire or trace, Fixed keeps the first instance's turn, and Upright stands them on the scene's up with only their heading following; along a curve they never flip or twist.
- A line can follow a trace, each copy tilted to the surface under it and standing on the mesh's smooth normals, including on a mesh imported as a triangle soup. A closed trace spaces the copies around the whole loop without doubling up at its start.
- A trace on clay, or on the body being repeated, cannot place copies, and the card says why.
- A ring turns about its pin's up, with its center at the pin and its handle on the first instance. It takes a Pitch as well as a Count: one instance every so far along its arc, as many as fit.
- A ring can start toward a pin. Add Pin puts one where the first instance stands, and dragging it around the ring's center turns the whole ring; a pin on the ring's axis says it points nowhere.
- A ring about a wire is a spiral, whose instances advance along the wire while they turn about it. The Array tool's Spiral makes the wire and its pins; Arc runs past 360° for several turns, Progress sets how much of the wire the spiral covers, and a pitch is measured along the spiral.
- Symmetric centers a line or a grid on its locator, so a pin at the middle of a face spreads the copies evenly around it. An odd count puts one instance on the pin, and an even count straddles it. A line on a wire or a trace, and a ring, are placed by their own path or center, so Symmetric is off there.
- The card shows how far apart a counted line's instances are, or how many instances a pitch makes. It says when a pitch asks for more than the 1,024 instances an array holds.
- The Surface row, under Located by, lays an array on a mesh or a clay body. A Line, Ring or Grid is walked over the surface from the locator's nearest point, the pitch and the radius are distances along the surface, and each instance stands up along the normal there.
- On clay, the walk follows the clay's own field, including its displacement and its Booleans. Instances on a clay body lie on the body as it is before they cut or join it, so arrayed nuts can be combined into the body they stand on.
- A line located by a wire lies on the surface too: each instance goes to the nearest point of the body, stands up along its normal, and keeps its spacing along the wire. A spiral takes no surface, so choosing a spiral clears it.
- Lift stands instances off the surface, or sinks them into it when negative. Instances that run past a mesh's edge, or cannot find the clay's surface, are left out and counted on the card, and they return when the layout fits again.
- On a surface, the handles sit on the placed instances and measure along the surface, and a ring's outline runs through its instances. Drag the locating pin over the body with Move's Surface snap and the layout follows.
- An array whose surface depends on its own instances through a chain of Combine tools says so on its card.
- Variations change each copy: an animation frame of one of a rig's clips, a rotation, a scale, or a random jitter that stays the same for the same seed. Each steps from its first value or spreads over a range.
- A ring of a rig's figure with a frame step per copy shows the clip's frames side by side, as a zoetrope does, while the rig itself keeps its own pose. Animation Frame applies to rigs only, and the card says so on clay.
- A rotation turns about one axis, so add a row for each axis to turn about. Rows apply in the order listed, and Move up and Move down reorder them.
- A rotation can turn In Steps. Set the step angle, for example 90 degrees, and each copy takes a whole number of steps, in order or picked by a seed.
- Scale and Jitter can differ by axis. Unlink the axes with the link button and give X, Y and Z their own values; a jitter of 0 holds that axis still.
- A scale stops at 0.01, so a copy never goes flat, and the card says when a ramp reaches it. Clay stretched along one axis keeps its surface exactly.
- Instances can be switched off one by one, for a gap such as a gate in a row of fence posts. Select the arrayed object, then click an instance to switch it off, or its ghost to switch it back on. Shift-click switches a range from the last one you switched, and Alt-click selects the object without switching anything.
- The first click on an array that is not selected only selects it, so selecting never punches a hole.
- While an array is selected, each instance shows a checkbox at the middle of its box, and hovering one outlines its instance. On a dense array the checkboxes thin out so none overlap, and zooming in shows more; they hide while you pick a target.
- Switched-off instances show as faint ghosts while the array is selected and are not drawn otherwise. Bake, Materialize Nodes and export leave them out, and each switch is its own undo step.
- The card's Switched Off list has a button on each entry to switch it back on, and hovering an entry outlines its instance; Switch All On clears the list. Lowering the count keeps switched-off instances listed as beyond the count, and raising it switches them off again.
- Changing the layout switches every instance back on. Switching Symmetric keeps them, and each moves with the layout.
- Switching instances off works for meshes, rigs and clay. Copies follow Smooth shading, Zebra stripes and Wireframe as the original does.
- On clay, each instance draws as a body of its own, up to 1,024 instances. Instances that touch meet with a crease, as separate clay nodes do, and the body's Round rounds each one.
- Clay arrayed and taken whole by another body as a Boolean tool holds only as many instances as that cut can evaluate, and fewer for scaled copies or shapes such as a dodecahedron. The card says how many show and why.
- A clay body's Boolean modifier can take one instance of an arrayed clay node as a tool. In the modifier's Add Clay picker, click an instance in the scene, or choose it from the Instances rows under the arrayed node in Objects; Alt-click picks the whole node.
- An instance tool's chip names the body and the instance and outlines it while you hover. It says why the tool cuts nothing: the instance is switched off, it is beyond the array's count, or its array is gone; the cut returns when the instance does.
- Deleting an arrayed node removes its instance tools, and Materialize Nodes points them at the nodes the instances became, so a cut keeps cutting.
- Materialize Nodes makes each copy a node of its own, in a group where the original stands, as one undo step. On clay, each copy becomes a clay node.
- Bake on an arrayed clay node meshes one instance and places a copy of that mesh at each instance that is on, as mesh nodes in a group, and the array's pins and wires go with it. The Bake tool bakes the arrays in the selection.
- Copies posed at a frame of a rig's clip bake to plain meshes in that pose, and the rig stays, hidden, with its clips.
- Export without baking writes every copy as Bake would make them. An arrayed clay node exports as one clay node per instance, and copies of the same size are meshed once, so a long array exports about as quickly as one instance.
- Skeletonize on an arrayed clay node builds its lattice from one instance and draws a copy at each instance, with no extra limit on the count.

### Hair

- The hair's Wall section is now Walls, one list of the wall nodes and the meshes its strands end at, in the order they were added. Each row shows how many strands landed on it.
- The Add wall menu makes a new hull, offset or arc wall, or points at one that exists. Existing wall… takes a wall node, and Surface… takes a mesh from the dropdown or a click in the scene; a click on a face starts the wall on that face's group, and the dropdown on the whole mesh.
- A mesh's row has Target faces chips to limit the wall to some of its face groups. A wall node's row folds its shape settings behind a chevron and says when other hair shares it.
- A hair takes one offset or hull wall, and the Add wall menu says so when it has one.
- Strands turn to reach a mesh. The strands that see it turn together by one angle, so strands from a flat face stay parallel and never cross, and they swing round with the mesh when you drag it. Each mesh row shows that angle in degrees.
- The angle is the middle of the range every strand can reach the mesh by, so the outermost strands stay off its edges. Where no one angle serves the whole hair, as with a mesh narrower than the hair, the angle most strands reach it by wins, and the others are flagged as having no target.
- Converge to fit, on a mesh's row, sends each strand to its own point on the mesh, in order along its row, so the strands cover the mesh's whole width and crowd toward their tips. The row shows the range of angles, and strands that collide are flagged.
- A strand sees only the front of a mesh, at its own height, within the maximum deflection and the maximum length. A mesh row nothing lands on says why: it is behind the hair, hidden by another mesh, farther than the maximum length, at no height the hair reaches, or seen from behind.
- A strand's tip reaches into a mesh by the wall penetration, no further than the mesh is thick where it lands. Against a sheet or an open shell it ends flush.
- A hair can run to several meshes. It is parted between them in the order they stand, each taking a run of the hair, so the strands spread apart and never cross, and every strand a mesh can take lands, even when each mesh is smaller than the hair.
- Each mesh row has a Share slider that weights how much of the hair it takes (0 leaves it none). Its Takes strands from row limits it to strands from some of the hair's own face groups.
- Where meshes differ in height, the hair is parted band by band. The rows where two bands meet are counted, since strands at different angles may cross there, and rows no mesh reaches are flagged with the reason.
- Wall nodes take what no mesh takes: the rows above the meshes and the strands no mesh sees. A strand that reaches nothing shows a red follicle and is counted under Reaching no surface wall.
- Strands that turn stand closer side by side, by the cosine of the turn, so a hair that turns far may need a wider column pitch.
- A deleted wall node, a deleted mesh, or face groups that no longer exist keep their row, with the reason and an unlink button, and build no strands.
- Dragging a mesh the hair runs to moves the strands with the drag, in a median of 19 ms on the tour's cubes, and moving one mesh recomputes only that mesh's strands. A hair that computes in a few milliseconds updates without waiting for edits to settle.
- On detailed meshes such as scans and sculpts, strands meet a simplified copy about one and a half strand pitches across, which keeps the outline of the chosen face groups. Exact targets, in Walls, makes them meet each mesh as it is, at the cost of recompute time.
- Hair saved by earlier versions opens with its walls unchanged.
- A wall can be split, leaving a gap of a set width at a set angle, so a wall around the whole part no longer closes into a ring that traps it. Strands that would land in the gap land beside it, and where the strands already leave a wider gap, the wall opens there.
- New hull walls start split, and walls in existing documents stay closed until you turn Split on. On an arc wall, the split works as a keep-out.
- Wall cone adds a strain relief cone where each strand meets the wall, pointing back toward the root, with its own radius, taper and buried depth. With both cones on, together they taper over at most half the strand.
- A strand whose wall cone pokes out of the back of the wall is flagged, checked at the angle the strand meets the wall. The wall rises high enough to cover the cones on its top row.
- Each strain relief cone has a Flat option, off by default, that keeps it within its strand's layer, so it prints with nothing above or below the strand.
- A strand's tip has its own Wall penetration, apart from Surface penetration at its root. Existing hair keeps one value for both ends until you change one.
- A hair of thousands of strands shares its work across the computer's spare cores, with the same strands as on one core. 21,000 strands on a 100,000-triangle target recompute in about 140 ms when the target moves.

### Lattice

- The Pattern menu adds six repeating unit cells beside Truss and Voronoi: Cubic, Body-centered cubic, Face-centered cubic, Octet truss, Kelvin foam and Diamond. A unit cell repeats through the body, cut where it meets the surface, at the Cell size you set.
- A Pattern pin sets where the pattern starts and, by its north and up, which way it runs, so moving or turning the pin moves the pattern through the body. Scale with pin multiplies the cell spacing by the pin's scale.
- With a unit cell in a Shell mode, Shell spacing sets the spacing of the surface struts.

## Bake

Wiki: [Bake an SDF to a Mesh](Workflow-Bake-an-SDF-to-a-Mesh)

- Dual contouring no longer folds faces inward on steep displaced relief or on large clay bodies. Each vertex stays in its own cell, and each quad splits along the diagonal that keeps both triangles facing out.
- Surface nets gives parts thinner than its cells closed, printable edges, where two sheets passing through one cell had met at one vertex. Shapes without thin parts mesh as before.
- Smooth evens out the triangles over the whole surface of a rounded shape, where on a sphere it smoothed two opposite eighths and left the rest.
- The purple grid preview encloses the whole shape being meshed, including tools placed far outside the clay or inside scaled groups, and the growth from Round, Threshold and displacement. On an arrayed clay body, it draws a box at each instance that stands.

## Pin and Wire

Wiki: [Construction and Rigging](Tool-Construction-and-Rigging)

- Pins, Jig and Scribe placed on a stretched (non-uniformly scaled) body take the body's true surface normal.

## Move

Wiki: [Construction and Rigging](Tool-Construction-and-Rigging#move)

- A pin dragged with Surface snap stands out of the surface along its normal, on meshes and clay, and keeps its heading as it slides. An Array's pin snaps to other bodies and passes over the array it places.

## Crack and Scribe

Wiki: [Surface and Refine](Tool-Surface-and-Refine)

### Crack

- Bite radius sets how far a crack line keeps its heading before it turns, so a larger radius makes straighter cracks. It is a distance, so a dense mesh cracks like a coarse one, where a denser mesh made wavier cracks and closed fewer of them.
- The guide brush and the bite radius are one brush, shown as rings under the cursor, and the Brush size slider is gone. Holding Shift paints a guide with the same circle, and Shift+scroll resizes it in Group mode; the radius is in millimeters, and Crack remembers it.
- Falloff, under Bite radius, draws a crack toward soft creases and valleys past the radius plus the falloff, and blends in between. The cursor's outer ring marks where the pull is full, and Crack remembers the falloff.
- Selecting a crack in Group mode shows its 3 to 24 control points as small orange squares, fainter behind the surface and hidden while Shift paints a guide. Each crack row in the panel shows how many points it has.
- Drag a point to move it. Only the two stretches beside the point change, and the groups update when you let go.
- Click the selected crack to add a point, or press Delete to remove the selected point, or the crack when no point is selected. The , and . keys step through the points, and Escape clears the point, then the crack.
- When no path joins a moved point's neighbors, the point stays where it was, a red ring shows around it, and the status line says so. A closed crack keeps at least 3 points.
- Point edits undo and redo on their own while Crack is open, and Refine, Subdivide and Merge small groups reset that history.
- A crack that follows the boundary of a group the mesh already had shows no points and cannot be edited while Keep existing groups is on. Turn Keep existing groups off to edit it.
- Hovering a large mesh with many cracks keeps the computer responsive.

## Rig and Pose

Wiki: [Rig and Pose a Character](Workflow-Rig-and-Pose-a-Character)

### Timeline

- You can author clips. Pose a rig, then Key pose at playhead (the diamond-plus button, or the clip menu) writes the posed bones as keys into the clip shown, and New clip from pose starts a clip of your own, two seconds long until you trim it.
- Keys play back as the pose you keyed, and between keys the rig moves as in an imported clip. A bone you pose after a clip began stays at rest until its first key, and keying past the clip's end widens the clip.
- Keys show as diamonds under the scrubber, and the panel opens a little taller to fit them. Click a key to go to it, drag it to another frame, or Shift-click it (or press Delete) to delete it; Alt with the left and right arrows moves the key under the playhead one frame.
- The clip menu, the … button beside the clip name, renames, duplicates, trims, reorders and deletes a rig's clips.
- Trim sets the range a clip plays. Keys outside the range stay, so widening it brings them back, and Crop keys outside the range removes them while the clip plays the same inside it.
- Authored clips are saved with the document and export to GLB and FBX with the clips you imported. Every key and clip edit is one Undo step.

## Paint

Wiki: [Paint Decals and Texture Maps](Workflow-Paint-Decals-and-Texture-Maps)

### Texture maps

- The Equirectangular projection, the longitude-latitude layout planetary maps use, wraps an image around a body pole to pole, with no seam and no spikes at the poles. Its relief blurs the same distance in every direction on the sphere, so craters near the poles stay round.
- A clay texture map has a Color mode, which paints its image's colors on the body through the map's projection and changes no shape. One image can displace a body while another colors it, as a Moon displaced by LOLA heights and colored by an LROC albedo map.
- Project color, on a clay displacement map, paints its image's colors through the same projection, and at a Scale of 0 it paints alone. While it is on, the card shows the image pixel under the pointer, with its longitude and latitude on an equirectangular map, so you can check where each pixel lands.
- Color maps apply in the order of the modifier list, each painting over the colors before it where its image has alpha, so the last one shows on top. They sample smoothly by default, with Nearest for crisp pixels.
- Bake keeps clay's color maps as vertex colors, each vertex taking the color at its place. A planar map colors only the surfaces that face its image.
- In Displacement mode on clay, a Detail filter (on by default) makes Bake average relief finer than its cells before meshing, so the mesh does not fold over it. Filter width sets how much, from half a cell to two.
- A file holding a texture map or decal with a projection this version of Kelcite doesn't know opens without that modifier and says so.

## Objects and the interface

### Camera

- A diamond marks the point the view turns about while you orbit or zoom with the mouse, a touch gesture or a 3D mouse. It keeps one size on the screen, fades once the view rests, and flashes where a camera flight lands. Show camera pivot, in Preferences \> Controls, turns it off.
- Orbit about the point under the cursor, in Preferences \> Controls (off by default), makes a right-drag turn the view about the surface where the drag begins, and that point stays under the cursor. A drag over empty space turns about the orbit target, and the view does not jump when a drag begins.
- After a pan, the first orbit turns about whatever is at the center of the screen, without shifting the view; over empty space the pivot stays where it was. Re-center the pivot after panning, on by default, controls this, and off, an orbit after a pan turns about the previous pivot.
- After a reload, the view opens where you left it, centered on the point you were orbiting, instead of looking off to one side until the first zoom.

### Objects

- Press Cmd+F (Ctrl+F on Windows and Linux), or click the magnifier in the panel's header, to search the tree. It narrows to the objects whose names hold all your words, in any order and any case, with their groups kept above them and the matching letters marked.
- The up and down arrows move through the matches and select each one. Enter keeps the match selected, closes the search and frames the match as F does; Escape clears and closes the search without moving the camera.
- While a tool asks you to pick an object, the arrows only move through the matches, and Enter picks the match and leaves the camera where it is. The search stays quick in scenes of 20,000 objects.

### Selection

- Clicking a part near the edge of another selects the part under the cursor. In scenes with many small parts, a click could select nothing.

### Undo

- Imported history (Settings → Data) replays Duplicate, Group, Ungroup and Delete from the Objects panel and their keys. A step you undo and then redo keeps its place in exported history, where it was skipped.

## Files and the tour

Wiki: [Getting Started](Workflow-Getting-Started#the-demo-tour)

### The tour

- New stops for Array (three dice whose pips are arrays on the cube's faces, and a ball with a grid of studs), Hair surface wall (a cube of hair parted between two smaller cubes), Camera pivot and Object search.
- The Texture map stop is a plaster wall with a brick panel, raised by a Texture Map in Displacement mode on one painted face group. Its caption walks through painting an area, adding the modifier, choosing the image and Scale, and picking the group under Face Groups; Bake writes the relief into the mesh for printing.
- The Pose stop shows clip authoring and the clip menu, and the Crack stop shows reshaping a crack by its points.
