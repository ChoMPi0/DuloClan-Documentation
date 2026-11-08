# Window
A window floats above normal UI controls. It is focused and brought to the front when clicked or selected, can be closed with the escape input, and can be dragged with a [Drag Object](../miscellaneous/drag_object.md).

## Setup
1. Create an object under your canvas and add **UI Window** (Add Component -> UI -> Window). A CanvasGroup is added automatically.
2. Choose an **ID**. Pick **Custom** to enter a **Custom ID**.
3. Choose the **Starting State** (Shown or Hidden).
4. Choose the **Escape Key Action**.
5. Tick **Use Black Overlay** to dim the UI behind the window. This needs a [Black Overlay Manager](black_overlay_manager.md) or a [Black Overlay](black_overlay.md) in the canvas.
6. Choose the **Transition** and, for Fade, its **Easing** and **Duration**.
7. Open the window from a button with `Show()` or `Toggle()`, or use [Close And Show Window](close_and_show_window.md) or [Toggle Window And Tab](../miscellaneous/toggle_window_and_tab.md).

## Inspector Settings

| Property | Summary |
| --- | --- |
| ID | A predefined UIWindowID value. Default None. |
| Custom ID | An integer ID. Shown only when **ID** is Custom. When it is 0, a free ID is assigned on start. |
| Starting State | The visual state applied on awake. Default Hidden. |
| Escape Key Action | What the escape input does to this window. Default Hide. |
| Use Black Overlay | Shows the [Black Overlay](black_overlay.md) behind the window while it is open. |
| Allow Re-parenting | Lets focusing move the window to the canvas (or UI Scene content) root before bringing it to the front. Default on. |
| Transition | Instant or Fade. Default Instant. |
| Easing | The fade easing. Default InOutQuint. |
| Duration | The fade duration in seconds. Default 0.1. |

## Identification
Windows can be found with the static methods by **ID** or **Custom ID**.

[UIWindowID](window_id.md) values: None, Custom, Settings, GameMenu, ModalBox, Character, Inventory, SpellBook, Dialog, QuestLog, Vendor.

When **Custom ID** is 0 at start, it is set to `NextUnusedCustomID` (the highest custom ID among windows plus one).

## Visibility
Visibility is controlled by the CanvasGroup alpha. A hidden window does not block raycasts.

`Show()` and `Hide()` do nothing if the component is disabled or the object is inactive. `Show()` also focuses the window.

The Fade transition ignores the time scale, so it works while the game is paused.

`IsOpen` is the target state. `IsVisible` is true while the alpha is above 0, so it is also true during a fade out.

## Escape Key Action

| Value | Summary |
| --- | --- |
| None | The escape input is ignored. |
| Hide | Hides the window when it is open. |
| HideIfFocused | Hides the window only when it is open and focused. |
| Toggle | Hides the window when open. Shows it when closed, if no window was hidden by the same press. |

The escape input is read by the [Window Manager](window_manager.md). A manager is created automatically on start when the window's action is not None and no manager exists. Escape is ignored while a [Modal Box](../modal_box/modal_box.md) is open.

## Focus
Clicking a window or selecting it in the EventSystem focuses it. Only one window is focused at a time; `UIWindow.FocusedWindow` returns it.

Focusing brings the window to the front with [UIUtility.BringToFront](../miscellaneous/utility.md). Objects with [Always On Top](../miscellaneous/always_on_top.md) stay above it.

## Black Overlay
With **Use Black Overlay** on, the window uses the overlay in its canvas, or creates one from the [Black Overlay Manager](black_overlay_manager.md). The overlay stays up while at least one window using it is open.

A window disabled or deactivated while open releases the overlay. It claims it again when re-enabled while still open.

## Dragging
Add a [Drag Object](../miscellaneous/drag_object.md) to the title bar and set its **Target** to the window.

## Events
**On Transition Begin** passes the window, the target state and whether the transition is instant. **On Transition Complete** passes the window and the state.

```
public void OnTransitionBegin(UIWindow window, UIWindow.VisualState state, bool instant)
{
    Debug.Log(window.name + " is transitioning to " + state);
}

public void OnTransitionComplete(UIWindow window, UIWindow.VisualState state)
{
    Debug.Log(window.name + " is now " + state);
}
```

## Object Oriented Methods

| Name | Returns | Summary |
| --- | --- | --- |
| Show() | void | Shows the window with its transition. |
| Show(bool) | void | Shows the window. Pass true to show it instantly. |
| Hide() | void | Hides the window with its transition. |
| Hide(bool) | void | Hides the window. Pass true to hide it instantly. |
| Toggle() | void | Hides the window if open, otherwise shows it. |
| Focus() | void | Focuses the window and brings it to the front. Does nothing if already focused. |
| BringToFront() | void | Brings the window to the front (uses **Allow Re-parenting**). |
| ApplyVisualState(UIWindow.VisualState) | void | Instantly applies the state without raising events. |
| SetCanvasAlpha(float) | void | Sets the canvas group alpha. |
| StartAlphaTween(float, float, bool) | void | Tweens the alpha to a target over a duration, optionally ignoring the time scale. |

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| ID | UIWindowID | The window ID. |
| CustomID | int | The custom window ID. |
| escapeKeyAction | UIWindow.EscapeKeyAction | The escape key action. |
| useBlackOverlay | bool | Use the black overlay. Can be changed at runtime, also while open. |
| focusAllowReparent | bool | Allow re-parenting on focus. |
| transition | UIWindow.Transition | Instant or Fade. |
| transitionEasing | TweenEasing | The fade easing. |
| transitionDuration | float | The fade duration in seconds. |
| IsOpen | bool | Is the window shown (or transitioning to shown). |
| IsVisible | bool | Is the canvas group alpha above 0. |
| IsFocused | bool | Is this the focused window. |

## Static Methods

| Name | Returns | Summary |
| --- | --- | --- |
| GetWindows() | List&lt;UIWindow&gt; | Gets all windows whose object is active in the hierarchy (disabled components are included). |
| GetWindow(UIWindowID) | UIWindow | Gets the first window with the given ID, or null. |
| GetWindowByCustomID(int) | UIWindow | Gets the window with the given custom ID, or null. |
| FocusWindow(UIWindowID) | void | Focuses the window with the given ID. |
| SortByCustomWindowID(UIWindow, UIWindow) | int | Compares two windows by custom ID, for sorting lists. |

## Static Properties

| Name | Type | Summary |
| --- | --- | --- |
| FocusedWindow | UIWindow | The currently focused window. |
| NextUnusedCustomID | int | The highest custom ID among windows plus one (0 when there are no windows). |

## Code Usage
```
public void ToggleInventory()
{
    UIWindow window = UIWindow.GetWindow(UIWindowID.Inventory);

    if (window != null)
        window.Toggle();
}

public void OpenSettingsInstantly()
{
    UIWindow window = UIWindow.GetWindow(UIWindowID.Settings);

    if (window != null && !window.IsOpen)
        window.Show(true);
}

public void HideFocused()
{
    if (UIWindow.FocusedWindow != null)
        UIWindow.FocusedWindow.Hide();
}
```
