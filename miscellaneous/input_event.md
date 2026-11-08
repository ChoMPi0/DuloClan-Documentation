# Input Event
Invokes UnityEvents when an input from the Input Manager is held, pressed or released. Use it for hotkeys, such as opening a window with a key, without writing code.

## Setup
1. Add **UI Input Event** to any object.
2. Enter the **Input Name**. Inputs are defined in Edit -> Project Settings -> Input Manager.
3. Assign methods to **On Button**, **On Button Down** or **On Button Up**.

## Inspector Settings

| Property | Summary |
| --- | --- |
| Input Name | The Input Manager button name. When empty, no events fire. |

## Behavior
- **On Button** fires every frame while the input is held. **On Button Down** fires on press. **On Button Up** fires on release.
- Events are skipped while a Selectable other than one on this object is selected in the EventSystem, so typing in an input field does not trigger hotkeys. This also applies to a button that stays selected after it was clicked.
- If the input is the [Window Manager](../windows/window_manager.md) escape input and escape already hid a window this frame, events are skipped.
- If the input name is not defined, the component disables itself and logs a warning.

## Events

```
public void OnButton() { }

public void OnButtonDown() { }

public void OnButtonUp() { }
```

## Code Usage
The component has no public methods or properties. Configure it in the inspector and point its events at your methods, for example **Toggle()** on a [Window](../windows/window.md).

```
public void ToggleInventory()
{
    UIWindow window = UIWindow.GetWindow(UIWindowID.Inventory);

    if (window != null)
        window.Toggle();
}
```
