# Modal Box Manager
A ScriptableObject that holds the [Modal Box](modal_box.md) prefab, creates boxes and keeps track of the open ones.

## Setup
1. Right click in the Project view and select Create -> UI Managers -> Modal Box Manager.
2. Save the asset in a **Resources** folder and keep the default name **ModalBoxManager**. It is loaded with `Resources.Load("ModalBoxManager")`, so a renamed asset or one outside a Resources folder is not found.
3. Assign the [Modal Box](modal_box.md) prefab to **Modal Box Prefab**.

## Inspector Settings

| Property | Summary |
| --- | --- |
| Modal Box Prefab | The prefab instantiated by `Create`. It must have a UIModalBox component. |

## Active Boxes
A box registers itself when shown and unregisters when confirmed or closed. Only the most recently shown box (`IsTopBox`) reacts to the confirm and cancel inputs.

## Methods

| Name | Returns | Summary |
| --- | --- | --- |
| Create(GameObject) | UIModalBox | Instantiates the prefab in the canvas of the given object (the object itself or a parent). Returns null if no prefab is assigned, the object is null or no canvas is found. The box is not shown until you call `Show()`. |
| IsTopBox(UIModalBox) | bool | Is the given box the most recently shown open box. |
| RegisterActiveBox(UIModalBox) | void | Registers a box as open. Used internally. |
| UnregisterActiveBox(UIModalBox) | void | Unregisters a box. Used internally. |

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| prefab | GameObject | The modal box prefab. |
| activeBoxes | UIModalBox[] | The open boxes, in the order they were shown. |

## Static Properties

| Name | Type | Summary |
| --- | --- | --- |
| Instance | UIModalBoxManager | The manager asset loaded from Resources, or null. |

## Code Usage
```
public void CreateAndShow()
{
    if (UIModalBoxManager.Instance == null)
    {
        Debug.LogWarning("Could not load the modal box manager while creating a modal box.");
        return;
    }

    UIModalBox box = UIModalBoxManager.Instance.Create(this.gameObject);
    if (box != null)
    {
        box.SetText1("Text line 1");
        box.SetText2("Text line 2");
        box.SetConfirmButtonText("Confirm");
        box.SetCancelButtonText("Cancel");
        box.Show();
    }
}

public bool IsAnyModalBoxOpen()
{
    return UIModalBoxManager.Instance != null && UIModalBoxManager.Instance.activeBoxes.Length > 0;
}
```
