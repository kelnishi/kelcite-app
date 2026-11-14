# Changelog 0.10.22

Released 2026-10-15.

A **demo scene with a tour** of every tool arrives on first launch, tooltips explain each tool with a picture, and any node can keep **Markdown notes**.

## Objects and the interface

- Each toolbar tool's tooltip explains what it does, with a picture; an info icon on the modifier panel explains modifiers the same way, and a tool that can't open says why
- Any node can keep Markdown notes in the Info panel and show them as a flag above it in the scene; Preferences sets the flags' size
- The Asset Library keeps assemblies: a group, a clay body, or a mesh with its pins, parts or modifiers, added from the Objects panel's menu, which comes back whole where it is dropped

## Files and the tour

Wiki: [Getting Started](Workflow-Getting-Started#the-demo-tour)

- A demo scene arrives in the document library on first launch, in your language, with a tour of every tool. The start screen and the document list link to it, and the tour plays from an overlay, flying the camera to each stop
- glTF export includes a ready-to-view copy of clay, hair and lattices for other apps, can compress the geometry, and writes face groups as standard glTF parts
- Node names reopen as saved, and every import gets new IDs, so a file imported twice makes independent copies
