# Mask Raycast Filter
Limits the clickable area of an **Image** to its visible pixels. Clicks on pixels below the alpha threshold pass through. Use it for irregular shapes such as map regions or ornate buttons.

## Setup
1. Add **Mask Raycast Filter** to an object with an **Image**.
2. Set the **Alpha Threshold** (0-1). Pixels with a lower alpha are not clickable.
3. Enable **Read/Write** in the import settings of the sprite's texture.

If the texture is not readable, a warning is logged and the whole rectangle stays clickable.

The filter uses Unity's built-in Image alpha hit testing, so it supports Simple, Sliced, Tiled and Filled images, trimmed and atlas packed sprites (Tight packing is not supported by Unity) and pixels per unit settings.

Note: Read/Write keeps a copy of the texture in memory. Use the [Circular](circular_raycast_filter.md) or [Rectangular](rectangular_raycast_filter.md) filter when a simple shape is enough.

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| alphaThreshold | float | Minimum alpha (0-1) of a clickable pixel. |

Disabling the component restores the Image's default behavior.
