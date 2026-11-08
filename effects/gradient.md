# Gradient
A vertical two-color gradient for UI graphics (Image, legacy Text, ...).

## Setup
Add **Gradient** to an object with a Graphic and set the **Top Color** and **Bottom Color**. The colors are multiplied with the graphic's own color.

The gradient spans the height of the generated mesh: for a Text it covers all lines together, not each line.

## TextMeshPro
Gradient does not affect TextMeshPro texts. Use **Color Gradient** in the TMP text component instead.

## Note on the Name
The class is `DuloGames.UI.Gradient`, which has the same name as `UnityEngine.Gradient`. If a script uses both namespaces, write the full name (`DuloGames.UI.Gradient`) to avoid the "ambiguous reference" error.
