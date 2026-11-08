# Toggle Window And Tab
Opens a [Window](../windows/window.md) on a specific [Tab](../controls/tab.md), or closes it when that tab is already showing. Use it for hotkeys or buttons that open one page of a tabbed window, such as a character window.

## Setup
1. Add **UI Toggle Window And Tab** to any object, usually the button.
2. Assign the **Window**.
3. Assign the **Tab** inside that window.
4. Add **Toggle()** to the button's **On Click** event, or to an [Input Event](input_event.md).

## Inspector Settings

| Property | Summary |
| --- | --- |
| Window | The window to show or hide. |
| Tab | The tab to activate. |

## Behavior
- If the window is open and the tab is on, the window is hidden.
- Otherwise the window is shown and the tab is activated.
- If either field is empty, nothing happens.

## Methods

| Name | Returns | Summary |
| --- | --- | --- |
| Toggle() | void | Shows the window on the tab, or hides it if already open on the tab. |

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| window | UIWindow | The window. |
| tab | UITab | The tab. |

## Code Usage
```
public UIToggleWindowAndTab spellBookToggle;

void Update()
{
    if (Input.GetKeyDown(KeyCode.P))
        this.spellBookToggle.Toggle();
}
```
