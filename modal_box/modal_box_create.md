# Modal Box Create
Creates and shows a [Modal Box](modal_box.md) from inspector settings, without code. It uses the [Modal Box Manager](modal_box_manager.md) to create the box.

## Setup
1. Make sure the [Modal Box Manager](modal_box_manager.md) exists and has a prefab assigned.
2. Add **UI Modal Box Create** (Add Component -> UI -> Modal Box Create) to an object under a canvas, usually the button that opens the box.
3. Fill in **Text 1** and **Text 2**.
4. Fill in **Confirm Text** and **Cancel Text**.
5. Assign a Button to **Hook To Button**, or call **CreateAndShow()** from an event or code.
6. Assign **On Confirm** and, if needed, **On Cancel**.

## Inspector Settings

| Property | Summary |
| --- | --- |
| Text 1 | The first text line. |
| Text 2 | The second text line (multi-line). |
| Confirm Text | The confirm button label. |
| Cancel Text | The cancel button label. |
| Hook To Button | A button that calls **CreateAndShow()** on click. The listener is added while this component is enabled. |

Every call creates a new box in the canvas of this object. The box is destroyed after it hides.

## Events
**On Confirm** and **On Cancel** are forwarded from the created box.

```
public void OnConfirm()
{
    Debug.Log("Confirm pressed.");
}

public void OnCancel()
{
    Debug.Log("Cancel pressed.");
}
```

## Methods

| Name | Returns | Summary |
| --- | --- | --- |
| CreateAndShow() | void | Creates a modal box with the configured texts and shows it. Logs a warning if the manager cannot be loaded. |
| OnConfirm() | void | Invokes **On Confirm**. Called by the created box. |
| OnCancel() | void | Invokes **On Cancel**. Called by the created box. |

## Code Usage
```
public UIModalBoxCreate deleteConfirmation;

void Start()
{
    this.deleteConfirmation.onConfirm.AddListener(DeleteItem);
}

public void AskToDelete()
{
    this.deleteConfirmation.CreateAndShow();
}

private void DeleteItem()
{
    Debug.Log("Item deleted.");
}
```
