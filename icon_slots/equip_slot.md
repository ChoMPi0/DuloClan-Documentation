# Equip Slot
An extension of [Slot Base](slot_base.md) designed for items, in addition this slot has **Equip Type** property.

## Tooltip
In order to change the equip slot tooltip you have to locate and change the method **PrepareTooltip(UIItemInfo itemInfo)** inside the [Item Slot](item_slot.cs) cs file.

## Events
The slot has three events, **onAssign**, **onAssignWithSource** and **onUnassign**.

Here is an example of eligible methods for the events.

```
public void OnAssign(UISpellSlot slot) { }

public void OnAssignWithSource(UISpellSlot slot, Object source) { }

public void OnUnassign(UISpellSlot slot) { }
```

## Code Usage
You can access any slot via the static methods **GetSlots()**, **GetSlotWithType(UIEquipmentType type)** and **GetSlotsWithType(UIEquipmentType type)**.

Here is an example of how to get and assign a item slot by id and group.

```
public void TestAssignSlot(UIEquipmentType type)
{
    UIEquipSlot slot = UIEquipSlot.GetSlotWithType(type);
    
    if (slot == null || UIItemDatabase.Instance == null)
        return;

    slot.Assign(UIItemDatabase.Instance.GetByID(1));
}
```

You can implement unequip when thrown away by unchecking the property **Allow Throw Away** and overriding the method **OnThrowAwayDenied()**.

```
/// <summary>
/// This method is raised when the slot is denied to be thrown away and returned to it's source.
/// </summary>
protected override void OnThrowAwayDenied()
{
    if (!this.IsAssigned())
        return;

    // Find free inventory slot
    List<UIItemSlot> itemSlots = UIItemSlot.GetSlotsInGroup(UIItemSlot_Group.Inventory);

    if (itemSlots.Count > 0)
    {
        // Get the first free one
        foreach (UIItemSlot slot in itemSlots)
        {
            if (!slot.IsAssigned())
            {
                // Assign this equip slot to the item slot
                slot.Assign(this);

                // Unassing this equip slot
                this.Unassign();
                break;
            }
        }
    }
}
```
