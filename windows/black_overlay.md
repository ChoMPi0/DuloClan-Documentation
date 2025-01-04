# Black Overlay
A simple overlay used behind windows.

##Simple Usage
Attach the component to any GameObject.

Create a prefab of it and assign it to the [Black Overlay Manager](black_overlay_manager.md).

The visibility of the overlay is controlled by a **CanvasGroup** alpha value.

To use the overlay with a [Window](window.md), select the window and tick the **Use Black Overlay** checkbox.

##Code Usage
You can access the black overlay at any time by the **GetOverlay(GameObject relativeGameObject)** static method.

```
public void ShowBlackOverlay()
{
    // Get an overlay in the current canvas
    UIBlackOverlay overlay = UIBlackOverlay.GetOverlay(this.gameObject);
    
    // Show it
    if (overlay != null)
        overlay.Show();
}
```
