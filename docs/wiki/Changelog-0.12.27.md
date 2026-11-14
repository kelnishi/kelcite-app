# Changelog 0.12.27

Released 2026-10-19.

**Texture maps displace clay**, spherical and cylindrical maps project from the body's center, Bake's Detail meshes relief finely, and modifiers can be **reordered**.

## Clay

Wiki: [Sculpt with Clay](Workflow-Sculpt-with-Clay)

- Texture maps displace clay: the surface rises by the image's brightness, live in the view and in Bake. Dropping an image onto clay adds it with a displacing texture map

## Modifiers

Wiki: [Modifiers](Tool-Modifiers)

- Modifiers can be reordered by dragging their grip, or with ↑ and ↓ on a focused grip; they apply from top to bottom. The demo tour has a Displacement stop

## Bake

Wiki: [Bake an SDF to a Mesh](Workflow-Bake-an-SDF-to-a-Mesh)

- Bake's Detail meshes sharp relief finely and flat areas coarsely, and texture maps have Sampling and Blur, so surfaces show no pixel steps

## Paint

Wiki: [Paint Decals and Texture Maps](Workflow-Paint-Decals-and-Texture-Maps)

- Spherical and Cylindrical texture maps project from the body's center onto the image and leave the surface alone off the image; Edge Blend fades the relief in from the image's edge
