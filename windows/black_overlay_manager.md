# Black Overlay Manager
This is a scriptable object which handles the instantiation of [Black Overlay](black_overlay.md) used by the [Window](window.md) component.

## Simple Usage
The manager must be placed in a Resources directory in your project.

To create the manager right click in your Project view then select Create -> UI Managers -> Black Overlay Manager.

Assign the [Black Overlay](black_overlay.md) prefab.

## Code Usage
You can access the black overlay manager at any time by the Instance static property.

```
public UIBlackOverlay GetBlackOverlay(GameObject relativeGameObject)
{
    // Find the black overlay in the current canvas
    Canvas canvas = UIUtility.FindInParents(relativeGameObject);

    if (canvas != null)
    {
        // Try finding an overlay in the canvas
        UIBlackOverlay overlay = canvas.gameObject.GetComponentInChildren();

        if (overlay != null)
            return overlay;

        // In case no overlay is found instantiate one
        if (UIBlackOverlayManager.Instance != null && UIBlackOverlayManager.Instance.prefab != null)
            return UIBlackOverlayManager.Instance.Create(canvas.transform);
    }

    return null;
}
```
