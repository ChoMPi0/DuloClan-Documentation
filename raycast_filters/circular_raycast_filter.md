# Circular Raycast Filter
Limits the clickable area of a UI element to a circle. Use it for round buttons, minimap frames or radial menus, so clicks on the transparent corners pass through.

## Setup
Add **Circular Raycast Filter** to an object with a raycast target Graphic (Image, RawImage, ...).

| Property | Summary |
| --- | --- |
| Radius In Percents | When on, the radius is a percentage (0-100) of the distance from the rect center to its corner (100 covers the whole rect); when off, it is in pixels. |
| Radius | The circle radius. |
| Offset | Moves the circle center away from the rect center. |

The circle is drawn as a gizmo in the Scene view when the object is selected.

Works with rotated and scaled elements and with all canvas render modes.

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| radiusInPercents | bool | Radius unit. |
| radius | float | The radius. |
| offset | Vector2 | Center offset. |
| center | Vector2 | The circle center in local space (read only). |
| operationalRadius | float | The radius in pixels after the percent conversion (read only). |
