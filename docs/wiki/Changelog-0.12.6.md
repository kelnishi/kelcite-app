# Changelog Feature Release 0.12.6

Released 2026-10-17.

The **Rig** tool rigs a figure for the Pose tool, by hand or with **auto-rigs**, binding weights with brushes, and rigged figures export to FBX and glTF.

## Rig and Pose

Wiki: [Rig and Pose a Character](Workflow-Rig-and-Pose-a-Character)

- The Rig tool (I) rigs a figure so the Pose tool can pose it: place joints by hand, bind the meshes and paint their weights, or run the auto-rigs
- The humanoid auto-rig places a skeleton from a few markers clicked on the figure, and the generic auto-rig finds the limbs of a creature, a robot or any other figure. Every choice they make has a control to correct it
- Bind solves the weights and classes each part as body, clothing or rigid; brushes add, subtract, replace and smooth weights, and test poses preview the result
- Rigged figures export to FBX and glTF for Unity, Blender and Daz Studio. Weight colors are set in Preferences, and the demo tour has a Rig stop
