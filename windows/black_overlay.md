# Black Overlay
A full screen overlay that dims the UI behind a [Window](window.md). Windows with **Use Black Overlay** show it while they are open.

## Setup
1. Create a full screen object (for example a black Image) and add **UI Black Overlay**. A CanvasGroup is added automatically.
2. Make it a prefab and assign it to the [Black Overlay Manager](black_overlay_manager.md). You can also place the overlay directly in your canvas.
3. Select a window and tick **Use Black Overlay**.

## Behavior
Each canvas uses one overlay. A window looks for an overlay under its canvas, and creates one from the manager when none is found.

The overlay hides itself on start and does not block raycasts while hidden.

When a window using it opens, the overlay is brought to the front, then the window above it. It fades with the window's **Easing** and **Duration** (instant for Instant windows) and ignores the time scale.

The overlay counts the windows using it. It hides only when the last one closes. A window disabled while open releases its count.

## Methods

| Name | Returns | Summary |
| --- | --- | --- |
| Show() | void | Shows the overlay instantly. Does not change the window count. |
| Hide() | void | Hides the overlay instantly. Does not change the window count. |
| IsActive() | bool | Is the component enabled and its object active in the hierarchy. |
| IsVisible() | bool | Is the alpha above 0. |
| SetAlpha(float) | void | Sets the canvas group alpha. |
| OnTransitionBegin(UIWindow, UIWindow.VisualState, bool) | void | Hooked to a window's **On Transition Begin** event. Windows with **Use Black Overlay** add it automatically. |

## Static Methods

| Name | Returns | Summary |
| --- | --- | --- |
| GetOverlay(GameObject) | UIBlackOverlay | Gets the overlay in the canvas of the given object, creating one from the manager if needed. Returns null if there is no canvas or no overlay could be created. |

## Code Usage
```
public void ShowBlackOverlay()
{
    // Get the overlay in this object's canvas
    UIBlackOverlay overlay = UIBlackOverlay.GetOverlay(this.gameObject);

    if (overlay != null)
        overlay.Show();
}
```
