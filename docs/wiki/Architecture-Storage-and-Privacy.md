# Architecture: Storage and Privacy

Kelcite has no server-side storage, no account and no analytics. Your documents never leave your machine unless you export or save a file yourself. That choice follows from the app [running entirely in the browser](Architecture-Overview).

The flip side is that the browser is the only place a document lives until you save it as a file. Browser storage is durable in normal use, but it belongs to the browser profile. Clearing site data for Kelcite, or using a private window, removes it.

![The open document autosaves to IndexedDB inside the browser profile; requests come in from Kelcite's site, optional downloads, asset sources and a SpaceMouse bridge; a document leaves only as a saved file](images/arch-storage-and-privacy.svg)

## Where documents live

The document list on the start page is read from the browser's IndexedDB storage. Each document keeps several records there.

| Record | What it holds |
|---|---|
| Index entry | Name, thumbnail, created and updated times, and the app version that last wrote it |
| Session | The scene and the current selection |
| Undo history | One snapshot per undo step, so undo survives a reload |
| Geometry and images | Mesh arrays and pictures, stored once and shared by the session and the undo history |

A second local database caches the distance fields that let a mesh act as [clay](Architecture-Geometry-Engine). Those fields can be rebuilt from the mesh at any time, so the cache stays outside the document and out of every save and export.

Two more local databases sit beside the documents. The asset library keeps the models, pictures and fonts you add to it, available in every document. The pasteboard keeps what you copy, so you can paste it into another document. When the browser's storage cannot be read, an alert and a line at the top of the Asset Library panel give the reason, since an empty panel would otherwise look like an empty library.

Preferences such as units, key bindings, panel layout, the camera's last view and asset library sources are kept in the browser's local storage, separately from documents.

## When Kelcite saves

Autosave writes the open document shortly after an edit, once edits pause. It also saves when you close the tab, when you leave the document, and before the app reloads to apply an update. Undo and redo record their own steps as they happen.

The browser copy needs no Save button. A file on disk is a separate step.

## Saving a file

Save writes a Kelcite GLB file — a standard glTF binary that also carries Kelcite's [scene data](Architecture-Scene-Model), so reopening it restores nodes, modifiers, layers and rigs. In browsers that let a page write files directly, Save keeps writing to the same file after the first time. In other browsers, Save opens the export dialog and the file arrives as a download.

Saving a GLB is the way to keep work that must outlive the browser profile, move it to another machine, or share it. Exports started from the document list report each document they could not write, in a banner above the list with the reason; Download All lists the documents it left out of the archive, and makes no archive when none can be exported. [Export](Workflow-Import-and-Export) to STL, 3MF, OBJ or FBX gives a file for other software, written in the [units](Architecture-Units-and-Files) each format expects.

## What leaves the browser

Kelcite makes network requests only for the app itself and for things you ask it to fetch. None of them carries your documents.

| Request | When | Goes to |
|---|---|---|
| The app and its updates | On load, and to check for a newer version (Preferences can turn the check off) | Kelcite's site |
| The demo document | When the demo document is first created, or replaced by a newer version | Kelcite's site |
| Optional downloads (the STEP importer, the rigging models) | Only when you install them from Downloads, after accepting their licenses | Kelcite's package host, or the publisher's own site where its terms do not allow Kelcite to host the file |
| Asset library catalogs, models, textures and fonts | When you browse or add from the asset library | The sources enabled in its settings, such as Poly Haven or Fontsource; each can be turned off |
| Deployment history | When you open the list of earlier versions | Kelcite's site |
| SpaceMouse input | When a SpaceMouse bridge is running | A service on your own machine |

Optional downloads are checked against the exact file the current version was tested with before they are used. Their licenses come from their own licensors, and Downloads lists each one with a link to its text.

Once the app has loaded, it works offline. Optional downloads and the asset library need a connection the first time they fetch something.
