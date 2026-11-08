# Nicer Outline
A smoother outline for UI graphics. Unity's **Outline** draws 4 offset copies; Nicer Outline draws 8, which gives rounder corners on thick outlines. It also scales the outline with **Best Fit** text.

## Setup
Add **Nicer Outline** to an object with a Graphic.

| Property | Summary |
| --- | --- |
| Effect Color | The outline color. |
| Effect Distance | The outline thickness on X and Y. |
| Use Graphic Alpha | Multiplies the outline alpha by the graphic's alpha. |

Nicer Outline doubles the vertex count of Unity's Outline; use it where the quality difference is visible (large or thick-outlined text).

It is not driven by the [Effect Transition](../transitions/effect_transition.md), which only supports Shadow and Outline.

## TextMeshPro
Nicer Outline does not affect TextMeshPro texts. Use the **Outline** settings of the TMP material instead ([Shadows and Outlines](../getting_started/textmeshpro.md#shadows-and-outlines)). The TextMeshPro Converter replaces Nicer Outline on texts with a material preset.
