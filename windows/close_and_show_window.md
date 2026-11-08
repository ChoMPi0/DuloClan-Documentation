# Close And Show Window
Hides one [Window](window.md) and shows another with a single method call. Use it for buttons that switch from one window to another, for example from the game menu to the settings.

## Setup
1. Add **UI Close And Show Window** to any object, usually the button.
2. Assign the window to hide to **Close Window**.
3. Assign the window to show to **Show Window**.
4. Add **CloseAndShow()** to the button's **On Click** event.

## Inspector Settings

| Property | Summary |
| --- | --- |
| Close Window | The window hidden first. Optional. |
| Show Window | The window shown after. Optional. |

## Methods

| Name | Returns | Summary |
| --- | --- | --- |
| CloseAndShow() | void | Hides **Close Window**, then shows **Show Window**. Empty fields are skipped. |

## Code Usage
```
public UICloseAndShowWindow switcher;

public void OpenSettings()
{
    this.switcher.CloseAndShow();
}
```
