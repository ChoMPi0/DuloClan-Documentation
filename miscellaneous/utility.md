# Utility
`UIUtility` is a static class with helpers used across the UI, for bringing objects to the front and finding components in parents.

## Bring To Front
`BringToFront` moves an object to the last sibling position so it is drawn above its siblings. [Windows](../windows/window.md) use it when focused.

1. The root is the content of the parent [UI Scene](../scene_system/scene.md), if there is one. Otherwise it is the parent canvas.
2. If re-parenting is allowed, the object is moved under the root, keeping its world position.
3. The object is set as the last sibling.
4. All active [Always On Top](always_on_top.md) objects under the root are moved to the end, sorted by order.

## Static Methods

| Name | Returns | Summary |
| --- | --- | --- |
| BringToFront(GameObject) | void | Brings the object to the front, re-parenting allowed. |
| BringToFront(GameObject, bool) | void | Brings the object to the front. The bool sets whether re-parenting is allowed. |
| FindInParents&lt;T&gt;(GameObject) | T | Finds a component of type T on the object itself or its closest parent. Returns null if none is found or the object is null. |

## Code Usage
```
public void ShowPopup(GameObject popup)
{
    popup.SetActive(true);

    // Keep it in its current parent, but draw it above its siblings
    UIUtility.BringToFront(popup, false);
}

public Canvas GetCanvas()
{
    // Checks this object first, then its parents
    return UIUtility.FindInParents<Canvas>(this.gameObject);
}
```
