# Ignore Raycast Filter
Makes a UI element and all of its children ignore raycasts, so clicks pass through to whatever is behind them.

## Setup
Add **UI/Raycast Filters/Ignore Raycast Filter** to the object.

Because Unity checks raycast filters on the hit graphic and all of its parents, every graphic under this object is ignored too.

## Alternatives
- To ignore a single graphic, untick **Raycast Target** on it.
- To ignore a whole hierarchy and be able to toggle it at runtime, use a **CanvasGroup** with **Blocks Raycasts** off.
