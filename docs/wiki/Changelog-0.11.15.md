# Changelog 0.11.15

Released 2026-10-16.

The **Pose** tool poses rigged characters, rigged FBX and glTF files keep their bones and clips, and a **Timeline** panel plays them. Bake's Pose operation turns a pose into ordinary meshes.

## Modifiers

Wiki: [Modifiers](Tool-Modifiers)

- Skeletonize has a rugby-ball icon, and the demo tour has a Pose stop

## Bake

Wiki: [Bake an SDF to a Mesh](Workflow-Bake-an-SDF-to-a-Mesh)

- Bake's Pose operation turns a rig's skinned meshes into ordinary meshes in their pose, with textures reprojected or fitted to a palette, and can make the figure one closed solid. It marks in orange where the pose makes the figure pass through itself

## Rig and Pose

Wiki: [Rig and Pose a Character](Workflow-Rig-and-Pose-a-Character)

- The Pose tool (E) poses a rigged character: pick a bone, turn it with the gizmo or typed angles, or drag a joint to pull its limb by IK while locked joints stay put. Pose both sides mirrors each edit, and poses can be flipped, copied, pasted and saved
- FBX files import with their units and axes, and a rigged FBX or glTF keeps its bones, skins, animation clips, textures and color maps
- A Timeline panel below the scene plays, scrubs and steps a rig's clips, and In place keeps a walking figure over its base
- Joints have rotation limits and stiffness, so elbows and knees hinge and a spine bends along its length

## Files and the tour

Wiki: [Getting Started](Workflow-Getting-Started#the-demo-tour)

- Rigged characters export to 3MF, OBJ and STL as posed. Tools that change a mesh wait until a skinned mesh's pose is baked
