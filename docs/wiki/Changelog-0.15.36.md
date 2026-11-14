# Changelog Bug Fix 0.15.36

Released 2026-10-28.

Recomputed normals and OBJ files **shade as the view shows**, and Pin's Loop mode follows edits.

## Trace

Wiki: [Construction and Rigging](Tool-Construction-and-Rigging#trace)

- On large models, Trace, Crack and Pin pick the face under the cursor or nearest a point, where they could land on a different face before.

## Pin and Wire

Wiki: [Construction and Rigging](Tool-Construction-and-Rigging)

- Pin's Loop mode finds loops on a model's current shape after you edit it, instead of the shape it had when you opened Pin.
- Changing Pin's crease angle in Loop mode now changes which loops it finds right away.
- Pin's center pin lands at the middle of the model's surface, including models inside a moved or rotated group and models with texture seams.

## Repair

Wiki: [Repair a Scan for Printing](Workflow-Repair-a-Scan-for-Printing)

- Recompute Normals in the Object Browser, and the Repair tool's fixes, now shade a model the way the view shows it: larger faces count for more, and a point where two face groups meet no longer takes the shading of just one of them. Recompute Normals is also several times faster on large models.

## Paint

Wiki: [Paint Decals and Texture Maps](Workflow-Paint-Decals-and-Texture-Maps)

- A wrapped decal on a selection whose edge touches itself at a corner, or around a hole, now covers the whole selection. Before, part of the image was cut off on one side of that corner and could show past the selection's edge.

## Files and the tour

Wiki: [Getting Started](Workflow-Getting-Started#the-demo-tour)

- OBJ files now carry the same shading the view shows: larger faces count for more, separate copies of one point smooth together, and models exported Z up with face groups no longer have their shading turned the wrong way.
