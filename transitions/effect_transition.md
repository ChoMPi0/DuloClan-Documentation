# Effect Transition
This component transitions the color of a **Shadow** or **Outline** effect with the state of a control, for example a glow outline that brightens on hover.

## Setup
1. Add a **Shadow** or **Outline** to the graphic.
2. Add **Effect Transition** to the control and assign the effect as its target.
3. Set the colors for **Normal**, **Highlighted**, **Selected** and **Pressed**, and the **Duration**.
4. Optionally enable **Use Toggle**, assign a **Target Toggle** and set the **Active Color** used while the toggle is on.

Only Unity's **Shadow** and **Outline** effects (and effects derived from them) are supported; [Nicer Outline](../effects/nicer_outline.md) is not.

Shadow and Outline don't work on TextMeshPro texts, so use this transition on Images. For a text glow, use a [Highlight Transition](highlight_transition.md) with **Text Color**, or swap TMP material presets from your own script.

See also [Highlight Transition](highlight_transition.md).
