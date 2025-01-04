# Modal Box
A simple modal box with text, cancel and confirm buttons.

## Simple Usage
Attach the component to any GameObject and assign the required fields.

After configuring your modal box create a prefab of it and assign it to the [Modal Box Manager](modal_box_manager.md).

The visibility of the modal box is controlled by a [Window](windows/window.md).

To instantiate a modal box use the [Modal Box Create](modal_box_create.md) component or via code.

## Events
The modal box has two events, **onConfirm** and **onCancel**.

Here is an example of eligible methods for the events.

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
        box.onConfirm.AddListener(OnConfirm);
        box.onCancel.AddListener(OnCancel);
        box.Show();
    }
}

public void OnConfirm()
{
    Debug.Log("Confirm pressed.");
}

public void OnCancel()
{
    Debug.Log("Cancel pressed.");
}
```
