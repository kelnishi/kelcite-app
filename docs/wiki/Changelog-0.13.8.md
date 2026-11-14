# Changelog Feature Release 0.13.8

Released 2026-10-20.

The **Mask** modifier clips images, **BitCanvas** images can be painted, clay takes decals, and Paint gains an **Airbrush**.

## Clay

Wiki: [Sculpt with Clay](Workflow-Sculpt-with-Clay)

- Clay takes Decal modifiers, as meshes do, and a decal can show an image or a BitCanvas as well as an SVG. Bake moves clay's decals onto the mesh it makes
- Clay displaced by an image follows each stroke on its mask as it is painted, without going flat in between

## Modifiers

Wiki: [Modifiers](Tool-Modifiers)

### Masks

- The Mask modifier clips an image to an SVG or another image laid over it, by alpha, a color channel or a chroma key. Everything that reads the image sees it masked, including texture maps, clay displacement and Bake

## Move

Wiki: [Construction and Rigging](Tool-Construction-and-Rigging#move)

- Move's surface snap snaps to clay and turns an SVG or image to face its surface. The demo tour has Mask, BitCanvas, Unproject and decal stops

## Paint

Wiki: [Paint Decals and Texture Maps](Workflow-Paint-Decals-and-Texture-Maps)

- Paint opened on an image paints it through a mask, adding a BitCanvas for the mask, and Paint's Unproject paints a BitCanvas on the clay or mesh it is projected onto
- An SVG decal stays sharp however far you zoom in; an SVG with gradients, filters or similar effects still draws from pixels, and its row says why
- Paint has an Airbrush with Flow, a brush preview in its panel, and a see-through dab in the brush's outline. While Paint is open, its panel lists the masks and texture maps that read the canvas, so they can be tuned while painting

## Objects and the interface

- The camera no longer jumps after a drag whose release it missed; pressing Undo twice quickly undoes two steps; clicking a dialog's dimmed backdrop closes it; 57 Info panel labels show in your language

### Images

- An image of fixed pixel size made with Convert to BitCanvas, in an image's menu in the Objects panel. Paint paints it with a brush, Shift-click erases and Alt-click picks a color, and Bake turns it back into an image

## Files and the tour

Wiki: [Getting Started](Workflow-Getting-Started#the-demo-tour)

- Exporting selected images writes PNG, JPEG or WebP files; scene exports to glTF, FBX and OBJ include images. Images and SVGs can hold other images and SVGs, which move with them
