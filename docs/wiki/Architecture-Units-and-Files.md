# Architecture: Units and Files

A 3D file is a list of numbers. Some formats say what one number is (a millimeter, an inch); many, including STL, do not. A model that is 50 units tall could be a 50 mm figurine or a 50 inch statue, and only the person who made it knows. Kelcite has to pick an interpretation, and it has to keep that interpretation stable when you save, reopen and export. That is why an imported model can come in at the wrong size, and why Preferences and the Inspector both have unit controls.

## The scene holds plain numbers

Kelcite stores lengths as plain numbers. Three settings in Preferences say how to read and show them. Each offers millimeters, centimeters, meters and inches, and each defaults to millimeters.

| Setting | What it decides |
|---|---|
| Scene unit | What one number in the scene is |
| Display unit | Which unit panels, readouts and the selection box's dimension labels show |
| Unit for files without one | What one number is in an STL, OBJ or PLY file, on import and on export |

The display unit changes only what you read. Switching it from millimeters to inches converts every readout and input field, and leaves the model's numbers untouched. Typed expressions (such as a constraint's distance) stay in the scene unit, because an expression can name other lengths in the scene.

These settings belong to your browser, and a document does not carry them. A Kelcite document records the scene unit it was saved with, so it opens at the right size in a browser set differently.

## A model can carry its own measure

A single scene can hold models made in different units, such as a part from a CAD file in inches beside a scan in millimeters. Each model can carry its own **measure** — the unit its numbers are read in. Its children inherit it. A model without one inherits its parent's, and at the top of the [scene tree](Architecture-Scene-Model), the scene unit.

The Inspector's Units row shows a model's measure and offers two actions.

| Action | What changes | When to use it |
|---|---|---|
| Choose a unit | How the model's numbers are read; its numbers stay the same, so its physical size changes | The model came in at the wrong size because its file had no unit or the wrong one |
| Commit | The model's numbers are multiplied into the measure it would inherit, so its physical size stays the same | You want the model to read like the rest of the scene from now on |

Commit multiplies every length the model holds: vertices, modifier distances, hair lengths, constraint offsets. Clay, images and SVGs get their size from their transform, so Commit scales the transform instead.

## How each format handles units

On [import](Workflow-Import-and-Export), a format that names its unit either tags the model with that unit or converts the model into the scene unit. CAD formats keep their numbers and tag the model, so a part designed in inches stays a round number of inches. Character and animation formats are converted, because [rigging and posing](Architecture-Rigging-and-Pose) work best when the whole figure shares the scene's measure.

| Format | Has a unit? | On import | On export |
|---|---|---|---|
| Kelcite GLB (native) | Yes | Restores each model's measure | Records the scene unit and each model's measure; other software reads it in meters |
| glTF / GLB from other software | Yes (meters) | Converted into the scene unit | Exported as Kelcite GLB |
| 3MF | Yes | Model tagged with the file's unit | Each model's measure multiplied out; unit written |
| STEP | Yes | Model tagged with the file's unit | Not exported |
| FBX | Yes | Converted into the scene unit | Written in millimeters, with the unit recorded |
| DAE, ZAE (Collada) | Yes | The importer applies the file's unit and brings the model in as meters, tagged as such | Not exported |
| STL, OBJ, PLY | No | Read in the unit for files without one, and tagged with it | Written in the unit for files without one (STL, OBJ) |
| Other mesh formats (3DS, AMF, X3D, DXF, IFC, Blender and more) | Not read | Read in the unit for files without one, like STL | Not exported |

![A length of 2 from a 3MF file in inches: stored as 2 with an inch measure, shown as 50.8 mm, exported to STL as 50.8, and stored as 50.8 after Commit](images/arch-units-and-files.svg)

A unit Kelcite has no setting for, such as microns or feet in a 3MF or STEP file, is converted to the nearest one it has.

A glTF or GLB file that Kelcite's own reader cannot open is tried with Assimp. If Assimp opens it, the model comes in with a warning that gives the first reader's error, because its materials, rig and extensions may be missing.

## Which axis is up

The scene's own frame has Y up, with the ground at zero height. Files disagree about up: many CAD and printing tools write Z up. The **Up Axis** preference (Y-Up or Z-Up, under Import / Export) says only which axis is up in the files it governs, such as STL and OBJ, and Kelcite turns the numbers on the way in and out. glTF files are always read Y-up, and 3MF files Z-up, as their formats define.

Inside the scene the setting changes nothing. An [Array](Architecture-Scene-Model) and a new [rig](Architecture-Rigging-and-Pose) stand along the scene's Y whatever it says.

## Exporting for printing

Exports for [printing](Workflow-Prepare-for-Printing) (STL, 3MF, OBJ) need plain triangles. A scene that still holds clay or unbaked modifiers reports which bodies need [Bake](Workflow-Bake-an-SDF-to-a-Mesh) before it writes the file.

A computed mesh can hold two copies of one point that differ only in their last digits. Export welds such corners: it rounds each position coordinate, in the file's unit, and each normal, color and texture coordinate to the nearest 0.0001, and corners that agree in all of them become one vertex in the file.

Exported normals are the ones the view shades with, so a vertex between two face groups takes each group's normal, and a rigged model's GLB shades smooth across its texture seams. FBX and OBJ export keep the normals an imported file brought until an edit changes the mesh or its normals. A saved document keeps computed normals, so a mesh reopened from one exports the view's.

The GLB export can also include a baked copy of the scene, so other software sees the finished shapes while Kelcite still reads the editable document. A GLB is also how a document [outlives the browser](Architecture-Storage-and-Privacy).
