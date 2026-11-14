# Changelog 0.17.406

Released 2026-11-14.

This release brings the **clay Loft**, a Loft that stays editable as clay, and **Ruled Lofts that rise evenly** from section to section, so a clay Loft and a mesh Loft of the same sections match. **Bake with Destination** joins a split's Loft into one closed body, and the **About panel** is reworked: its version links to this changelog, and it fits a phone.

## Clay

Wiki: [Sculpt with Clay](Workflow-Sculpt-with-Clay)

### Clay Loft

- A clay **Loft** joins two or more sections (SVG, text, closed Traces and closed Wires) along the same walls as a mesh Loft of those sections, at each section and between them, and redraws as its sections, **Blend**, **Twist**, **Start** and matching pins change. Its Loft card lists the sections and says when the clay can't follow them: a Pin section, a section that folds back over its take-off, or walls that pass through each other.
- A clay Loft follows its guide rails between sections, along the same walls as the mesh Loft. Along a spine, its card says that clay can't follow guide rails yet.
- Where a clay Loft's spine curves, or a Smooth turn has a Strength of 0, its card says how far the clay strays from the mesh Loft of the same sections.

### Surface snap

- **Surface** snap lands on clay bodies as well as meshes, as Move's does.
- **Surface** snap still lands a new primitive on the ground wherever it isn't over a body or clay, while the grid is shown.

## Loft

Wiki: [Loft Between Shapes](Workflow-Loft-Between-Shapes)

- A Ruled Loft with a Twist or a matching pin that turns it rises evenly from section to section, as its clay Loft does, so the two now match. Existing Ruled Lofts with a turn change shape slightly: their walls no longer leave each section lying flat along it. Smooth Lofts are unchanged.
- After baking Lofts and undoing the bakes, each Loft builds its body again instead of sometimes reporting "a Loft body takes one update at a time" until the scene was reloaded.

## Bake

Wiki: [Bake an SDF to a Mesh](Workflow-Bake-an-SDF-to-a-Mesh)

- A Loft between the two pieces of a split, baked with **Destination**, joins the destination piece along its Trace, with no cut face left inside; **Source** with **Destination** joins both pieces and the Loft into one closed body.
- A Loft whose walls pass into a body beside its Trace is baked as a mesh of its own, with an alert saying why, instead of being joined with its end cap left inside the body.

## Pin and Wire

Wiki: [Construction and Rigging](Tool-Construction-and-Rigging)

- A Wire saved without its pins' tangents opens with sharp corners; selecting it no longer closes the document and empties the scene.

## Jig

Wiki: [Construction and Rigging](Tool-Construction-and-Rigging#jig)

- Pressing another picker's crosshair while a Jig picker waits for a pick turns the Jig crosshair off, and removing a Jig item no longer cancels a pick another panel has open.

## Objects and the interface

### About

- The version under About's title is a blue link with a scroll icon. It opens what changed in the running release, on its page on the Kelcite wiki, in a new tab; a preview or development build opens the wiki's **Changelog** instead.
- About's version shows the number alone; a preview or development build shows its branch and commit on a line below.
- About no longer lists every release. **Check for updates** and **Looking for an old version?** sit under the version, and kelcite.app/changelog opens the release's page on the wiki.
- About has a title bar with a close button, so it closes on a phone, where tapping outside it does not. On a screen narrower than 640 px it fills the screen instead of running off both sides.
- About's **Open Source Software**, **Open Asset Sources** and **Optional Downloads** lists are collapsed, each showing how many entries it holds; click one to open it. **GitHub** and **Sponsor** describe where they go when you point at them, and the heart-hands corner, labeled **FAQ**, opens the FAQ.
- Enter on a link or button in About opens it, instead of closing About.

### Changelog

- Each release's notes are published as a page on the Kelcite wiki, grouped by the system they touch, with screenshots.
- kelcite.app/docs no longer has a changelog page; the wiki's Changelog replaces it.

## Rendering and performance

- Pin, Wire, Jig, Trace, Scribe, Repair, Paint, Array and the Rig weight brush no longer cast a ground shadow from their previews, handles and markers, and neither do Combine's faint cutter and ghost, committed Wires, or the preview grids and outlines that show while a slow preview works.
