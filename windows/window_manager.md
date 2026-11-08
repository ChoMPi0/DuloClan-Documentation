# Window Manager
The window manager reads the escape input and hides or shows [Windows](window.md) based on their **Escape Key Action**. One manager per scene is enough.

## Setup
1. No setup is required. When a window with an **Escape Key Action** other than None starts and no manager exists, a "Window Manager" object is created.
2. To change the input, add **UI Window Manager** to an object yourself and set **Escape Input Name**. It must be an input defined in Edit -> Project Settings -> Input Manager.

## Inspector Settings

| Property | Summary |
| --- | --- |
| Escape Input Name | The input that closes windows. Default "Cancel". |

## Behavior
When the escape input is pressed:

1. If a [Modal Box](../modal_box/modal_box.md) is open, nothing happens.
2. Every open window with the action Hide or Toggle, or HideIfFocused while focused, is hidden.
3. If at least one window was hidden, `escapedUsed` is true for that frame and nothing else happens.
4. Otherwise, every closed window with the action Toggle is shown.

[Input Event](../miscellaneous/input_event.md) components using the same input skip the frame when `escapedUsed` is true. This keeps one press from closing a window and opening a menu.

The flag is set and cleared in the manager's `Update`. Read it from code that runs after the manager, for example in `LateUpdate`.

Only one manager is kept. A duplicate removes itself, or its whole object when that object holds nothing else.

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| escapeInputName | string | The escape input name. |
| escapedUsed | bool | Was the escape input used to hide a window in this frame. |

## Static Properties

| Name | Type | Summary |
| --- | --- | --- |
| Instance | UIWindowManager | The current window manager, or null. |

## Code Usage
```
void LateUpdate()
{
    if (UIWindowManager.Instance != null && UIWindowManager.Instance.escapedUsed)
        return; // Escape already closed a window this frame

    if (Input.GetButtonDown("Cancel"))
        Debug.Log("Escape pressed with no window to close.");
}
```
