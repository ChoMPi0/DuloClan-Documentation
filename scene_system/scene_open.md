# Scene Open
This component opens a [UI Scene](scene.md) from a button or an input.

## Setup
1. Add **Scene Open** to a button (or any object).
2. Set **Action Type**: open a **Specific ID** or go back to the **Last Scene**.
3. Set **Scene Id** when using a specific ID.
4. Optionally assign **Hook To Button** to open the scene on click, and/or choose an **Input Key** (Submit, Cancel or Jump) to open it with an input.

The input is ignored while a [Modal Box](../modal_box/modal_box.md) is open or when Escape was already used to close a window this frame.

## Code Usage
Call `Open()` from a UnityEvent or code.

```
[SerializeField] private UISceneOpen m_OpenOptions;

public void OnOptionsPressed()
{
    this.m_OpenOptions.Open();
}
```
