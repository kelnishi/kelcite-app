# Changelog Feature Release 0.14.96

Released 2026-10-26.

Files Kelcite can't open or import **say why**, and kelcite.app/reset clears everything Kelcite keeps.

## Objects and the interface

- The picture in each tool's tooltip shows just that tool's example, without the neighboring examples around it

## Files and the tour

Wiki: [Getting Started](Workflow-Getting-Started#the-demo-tour)

- A file saved by a newer Kelcite that this version can't open says so, naming the version that saved it and pointing to Check for updates, where it said the file was damaged
- A file that can't be imported says why in your language: damaged or not of its type, holding nothing to bring in, too large for the browser's memory, or a kind this version can't read, one message per file, where it showed the reader's English or only a count
- Dropping a file of a type Kelcite doesn't open says so, instead of nothing happening
- In the tour's Pose stop, turning one of the figure's legs no longer drags the other foot with it
- Deleting a document always finishes and takes it off the list, where it could fail in a private window and leave the document listed until a reload
- Opening kelcite.app/reset clears everything Kelcite keeps in this browser, including the Asset Library, optional downloads, preferences and the saved panel layout, where it cleared only documents; it says when another Kelcite tab must be closed to finish
