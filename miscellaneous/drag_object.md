# Drag Object
Makes a UI element draggable. The drag starts on this object and moves a target, so a title bar can move its whole [Window](../windows/window.md). Supports inertia and keeping the target within the canvas.

## Setup
1. Add **UI Drag Object** (Add Component -> UI -> Drag Object) to an object with a raycast target Graphic, such as an Image (for example the window title bar).
2. Assign the RectTransform to move to **Target**. Without a target the component does nothing.
3. Untick **Horizontal** or **Vertical** to lock an axis.
4. Configure **Inertia** and **Constrain Within Canvas** if needed.

## Inspector Settings

| Property | Summary |
| --- | --- |
| Target | The RectTransform moved by the drag. |
| Horizontal | Allow horizontal movement. Default on. |
| Vertical | Allow vertical movement. Default on. |
| Inertia: Enable | Keep moving after release, slowing down over time. Default on. |
| Dampening Rate | How fast the inertia slows down. Higher stops sooner. Default 9. |
| Inertia Rounding | Hard rounds the position to even pixel values, Soft to whole pixels. Default Hard. |
| Constrain Within Canvas: Enable | Limit movement to the canvas. Default off. |
| Constrain Drag | Clamp the pointer position to the canvas while dragging. |
| Constrain Inertia | Block inertia movement on an axis while the target extends past a canvas edge on that axis. |

Constrain Drag clamps the pointer, not the target rect. The target can still extend past the canvas edge, but it stays reachable.

Inertia ignores the time scale, so it works while the game is paused.

## Events
**On Begin Drag**, **On End Drag** and **On Drag** pass the event data.

```
public void OnBeginDrag(BaseEventData data) { }

public void OnEndDrag(BaseEventData data) { }

public void OnDrag(BaseEventData data) { }
```

## Methods

| Name | Returns | Summary |
| --- | --- | --- |
| StopMovement() | void | Stops the inertia movement. |
| IsActive() | bool | Is the component active and a target assigned. |

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| target | RectTransform | The dragged target. |
| horizontal | bool | Allow horizontal movement. |
| vertical | bool | Allow vertical movement. |
| inertia | bool | Use inertia. |
| dampeningRate | float | The inertia dampening rate. |
| constrainWithinCanvas | bool | Constrain within the canvas. |

## Code Usage
```
public UIDragObject dragObject;

public void ResetWindowPosition()
{
    this.dragObject.StopMovement();
    this.dragObject.target.anchoredPosition = Vector2.zero;
}
```
