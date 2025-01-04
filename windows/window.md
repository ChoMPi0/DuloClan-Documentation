# Window
Windows float above normal UI controls, feature click-to-focus and can optionally be dragged around by the end user.

## Identification
You can assign an **ID** to the window and access it via the available static methods.

The **ID** can be predefined in the **UIWindowID** enum or a custom integer.

## Visibility
You can choose the default visibility state of the window by changing the property **Starting State** in the editor.

You can show, hide or toggle the window with the available object oriented methods.

The visibility of the overlay is controlled by a CanvasGroup alpha value.

## Black Overlay
You can choose to use the [Black Overlay](black_overlay.md) with the window from the editor or via code.

Make sure the [Black Overlay Manager](black_overlay_manager.md) exists and is configured.

## Dragging
To make the window draggable please refer to the [Drag Object](../miscellaneous/drag_object.md) component.

## Events
The window has two events **onTransitionBegin** and **onTransitionComplete**.

Here is an example of eligible methods for the events.

```
/// <summary>
/// Method for the window on transition begin event.
/// </summary>
/// <param name="window">The window.</param>
/// <param name="state">The window visual state that we are transitioning to.</param>
/// <param name="instant">If the transition is instant or not.</param>
public void OnTransitionBegin(UIWindow window, UIWindow.VisualState state, bool instant) { }

/// <summary>
/// Method for the window on transition complete event.
/// </summary>
/// <param name="window">The window.</param>
/// <param name="state">The window visual state that we are transitioning to.</param>
public void OnTransitionComplete(UIWindow window, UIWindow.VisualState state) { }
```

## Object Oriented Methods

| Name | Returns | Summary |
| ------------- | ------------- | ------------- |
| ApplyVisualState(UIWindow.VisualState) | void | Instantly applies the visual state. |
| BringToFront() | void | Brings the window to the front. |
| Focus() | void | Focuses this window. |
| Toggle() | void | Toggle the window Show/Hide. |
| Show() | void | Show the window. |
| Show(bool) | void | Show the window with Instant bool. |
| Hide() | void | Hide the window. |
| Hide(bool) | void | Hide the window with Instant bool. |
| IsActive() | bool | Determines whether this window is active. |
| SetCanvasAlpha(float) | void | Sets the canvas alpha. |
| StartAlphaTween(float, float, bool) | void | Starts alpha tween. |

## Static Methods

| Name | Returns | Summary |
| ------------- | ------------- | ------------- |
| FocusWindow(UIWindowID) | void | Focuses the window with the given ID. |
| GetWindow(UIWindowID) | UIWindow | Gets the window with the given ID. |
| GetWindowByCustomID(int) | UIWindow | Gets the window with the given custom ID. |
| GetWindows() | List<UIWindow> | Get all the windows in the scene (Including inactive). |
