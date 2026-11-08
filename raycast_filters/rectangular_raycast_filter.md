# Rectangular Raycast Filter
Limits the clickable area of a UI element to a smaller rectangle inside it. Use it when a sprite has a large transparent border or shadow that should not be clickable.

## Setup
Add **Rectangular Raycast Filter** to an object with a raycast target Graphic.

| Property | Summary |
| --- | --- |
| Offset | Moves the clickable rectangle. |
| Borders | Shrinks the rectangle from each side (left, right, top, bottom) in pixels. |
| Scale X / Scale Y | Scales the rectangle (0-1) relative to the element size. |

The rectangle is drawn as a gizmo in the Scene view when the object is selected.

Works with rotated and scaled elements and with all canvas render modes.

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| offset | Vector2 | Rectangle offset. |
| borders | RectOffset | Border insets. |
| scaleX | float | Horizontal scale (0-1). |
| scaleY | float | Vertical scale (0-1). |
| scaledRect | Rect | The resulting clickable rect in local space (read only). |
