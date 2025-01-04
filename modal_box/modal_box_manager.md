# Modal Box Manager
This is a scriptable object which handles the instantiation of [Modal Box](modal_box.md) and keeps track of the active boxes.

## Simple Usage
The manager must be placed in a Resources directory in your project.

To create the manager right click in your Project view then select Create -> UI Managers -> Modal Box Manager.

Assign the [Modal Box](modal_box.md) prefab.

## Code Usage
You can access the modal box manager at any time by the Instance static property.

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
```
