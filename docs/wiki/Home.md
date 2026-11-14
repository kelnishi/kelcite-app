<img src="images/icon.svg" alt="Kelcite" width="64">

Welcome to the [Kelcite](https://kelcite.app) wiki. Kelcite is a 3D modeling app for Makers, focused on 3D printing, that runs entirely in the browser.

Development happens in a private repository; this repository tracks the deployed app, feature requests and bug reports. Report a bug or request a feature in the [issue tracker](https://github.com/kelnishi/kelcite-app/issues), and see the [release notes](https://kelcite.app/docs/changelog/) for what changed in each version. You can support the project through [sponsorship](https://github.com/sponsors/kelnishi).

## New here

Start with [Getting Started](Workflow-Getting-Started), then [Import and Export](Workflow-Import-and-Export) to bring in a model, and [Prepare for Printing](Workflow-Prepare-for-Printing) when it is ready to print.

The app's own pages are on the web too. kelcite.app/faq opens the FAQ and kelcite.app/changelog opens About scrolled to the changelog. The FAQ, the changelog and each tool's tutorial are also plain web pages under kelcite.app/docs/, in English, each with a Markdown copy, so search engines and language models can read them without running the app; kelcite.app/llms.txt, sitemap.xml and robots.txt list them.

## How-to workflows

| Goal | Page |
|---|---|
| Open the demo, navigate, select | [Getting Started](Workflow-Getting-Started) |
| Bring models in and out, and handle units | [Import and Export](Workflow-Import-and-Export) |
| Fix a scan | [Repair a Scan for Printing](Workflow-Repair-a-Scan-for-Printing) |
| Sculpt with clay | [Sculpt with Clay](Workflow-Sculpt-with-Clay) |
| Turn clay into a mesh | [Bake an SDF to a Mesh](Workflow-Bake-an-SDF-to-a-Mesh) |
| Cut a model apart | [Split and Cut a Model](Workflow-Split-and-Cut-a-Model) |
| Build a solid through several outlines | [Loft Between Shapes](Workflow-Loft-Between-Shapes) |
| Paint, decals and texture maps | [Paint Decals and Texture Maps](Workflow-Paint-Decals-and-Texture-Maps) |
| Hair and strands | [Hair and Strands](Workflow-Hair-and-Strands) |
| Rig and pose a character | [Rig and Pose a Character](Workflow-Rig-and-Pose-a-Character) |
| Turn an SVG into a mesh | [Trace an SVG to a Mesh](Workflow-Trace-an-SVG-to-a-Mesh) |
| Assign spools to colors | [Manage Filaments](Workflow-Manage-Filaments) |
| Check a model before slicing | [Prepare for Printing](Workflow-Prepare-for-Printing) |
| Keys and preferences | [Keyboard Shortcuts and Preferences](Workflow-Keyboard-Shortcuts-and-Preferences) |
| Something went wrong | [Troubleshooting](Workflow-Troubleshooting) |

## Examples

[Moon Relief Test](Example-Moon-Relief-Test): lunar elevation and albedo wrapped onto a 100 mm clay sphere, with a bake at a million triangles as an option.

## Tool reference

[Create and Bake](Tool-Create-and-Bake) · [Cut and Join](Tool-Cut-and-Join) · [Surface and Refine](Tool-Surface-and-Refine) · [Construction and Rigging](Tool-Construction-and-Rigging) · [Modifiers](Tool-Modifiers)

## Architecture

How Kelcite is put together, at the level that explains its behavior. Start with [Overview](Architecture-Overview).

[Scene Model](Architecture-Scene-Model) · [Geometry Engine](Architecture-Geometry-Engine) · [Tools and Modifiers](Architecture-Tools-and-Modifiers) · [Units and Files](Architecture-Units-and-Files) · [Rigging and Pose](Architecture-Rigging-and-Pose) · [Storage and Privacy](Architecture-Storage-and-Privacy) · [Rendering](Architecture-Rendering) · [Performance](Architecture-Performance)
