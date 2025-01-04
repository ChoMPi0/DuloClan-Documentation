# Drag Object
This component makes any raycastable control draggable, you can also specify the dragging target.

## Simple usage
Attach the component to any GameObject with a Graphic component such as Image, Text etc.

Assign the target **RectTransform** that's going to be dragged.

## Events
The drag object component has three events, **onBeginDrag**, **onEndDrag** and **onDrag**.

Here is an example of eligible methods for the events.

```
public void OnBeginDrag(BaseEventData data) { }

public void OnEndDrag(BaseEventData data) { }

public void OnDrag(BaseEventData data) { }
```
