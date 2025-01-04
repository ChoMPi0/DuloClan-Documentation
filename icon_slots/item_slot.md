# Item Slot
An extension of [Slot Base](slot_base.md) designed for items, in addition this slot can have an **ID** and **Slot Group**.

## Tooltip
In order to change the item slot tooltip you have to locate and change the method **PrepareTooltip(UIItemInfo itemInfo)** inside the cs file.

## Events
The slot has five events, **onRightClick**, **onDoubleClick**, **onAssign**, **onAssignWithSource** and **onUnassign**.

Here is an example of eligible methods for the events.

```
public void OnRightClick(UISpellSlot slot) { }

public void OnDoubleClick(UISpellSlot slot) { }

public void OnAssign(UISpellSlot slot) { }

public void OnAssignWithSource(UISpellSlot slot, Object source) { }

public void OnUnassign(UISpellSlot slot) { }
```

## Code Usage
You can access any slot via the static methods **GetSlots()**, **GetSlotsWithID(int ID)**, **GetSlotsInGroup(UIItemSlot_Group group)** and **GetSlot(int ID, UIItemSlot_Group group)**.

Here is an example of how to get and assign a item slot by id and group.

```
public void TestAssignSlot(int id, UIItemSlot_Group group)
{
    UIItemSlot slot = UIItemSlot.GetSlot(id, group);
    
    if (slot == null || UIItemDatabase.Instance == null)
        return;

    slot.Assign(UIItemDatabase.Instance.GetByID(1));
}
```

You can implement item destruction confirmation by unchecking the property **Allow Throw Away** and overriding the method **OnThrowAwayDenied()**.

```
/// <summary>
/// This method is raised when the slot is denied to be thrown away and returned to it's source.
/// </summary>
protected override void OnThrowAwayDenied()
{
    if (!this.IsAssigned())
        return;

    if (UIModalBoxManager.Instance == null)
    {
        Debug.LogWarning("Could not load the modal box manager while creating a modal box.");
        return;
    }

    UIModalBox box = UIModalBoxManager.Instance.Create(this.gameObject);
    if (box != null)
    {
        box.SetText1("Do you really want to destroy \"" + this.m_ItemInfo.Name + "\"?");
        box.SetText2("You wont be able to reverse this operation and your item will be permamently removed.");
        box.SetConfirmButtonText("DESTROY");
        box.onConfirm.AddListener(Unassign);
        box.Show();
    }
}
```
