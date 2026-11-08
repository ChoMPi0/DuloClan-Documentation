# Item Slot
A [Slot Base](slot_base.md) that holds a `UIItemInfo`. Use it for inventory, bags, vendors and quest rewards. Slots are found by **Slot Group** and **Slot ID**.

## Setup
1. Set up the slot object as described in [Slot Base](slot_base.md).
2. Add **Item Slot** (Add Component -> UI -> Icon Slots -> Item Slot).
3. Choose the **Slot Group** (None, Inventory, Dialog, QuestLog, Vendor).
4. Set a **Slot ID** that is unique in the group.
5. Make sure the [Tooltip Manager](../tooltips/tooltip_manager.md) has the custom styles the item tooltip uses.

In the editor, a slot with a duplicate ID in its group gets a new ID automatically when enabled, and a warning is logged. You can also use **Auto Assign ID** from the component context menu.

## Properties

| Property | Summary |
| --- | --- |
| Slot Group | The group used by `GetSlotsInGroup` and `GetSlot`. |
| Slot ID | The slot ID within its group. |

All other properties come from [Slot Base](slot_base.md).

## Behavior
- **Swapping:** items swap with other item slots and with equip slots. A swap with an [Equip Slot](equip_slot.md) only happens when the item's equip type matches the slot.
- **Throw away confirm:** when **Allow Throw Away** is off, dropping the item outside a slot opens a [Modal Box](../modal_box/modal_box.md) asking to destroy it. **DESTROY** unassigns the slot. This needs the [Modal Box Manager](../modal_box/modal_box_manager.md).
- **Tooltip:** shown after **Display Delay** and anchored to the slot. It uses **Item Tooltip Width** from the manager and the custom styles **ItemAttribute**, **ItemStat** and **ItemDescription**. The title is colored by item quality.

To change the tooltip content, edit `UIItemSlot.PrepareTooltip(UIItemInfo)`, or override `OnTooltip(bool)` in a subclass.

## Events
| Event | Summary |
| --- | --- |
| onRightClick | Right click on an assigned slot. |
| onDoubleClick | Left double click on an assigned slot. |
| onAssign | The slot was assigned. |
| onAssignWithSource | The slot was assigned. Also passes the source slot (null when assigned from code). |
| onUnassign | The slot was unassigned. Also called at the start of every assign. |

Eligible methods:

```
public void OnRightClick(UIItemSlot slot) { }

public void OnDoubleClick(UIItemSlot slot) { }

public void OnAssign(UIItemSlot slot) { }

public void OnAssignWithSource(UIItemSlot slot, Object source) { }

public void OnUnassign(UIItemSlot slot) { }
```

## Methods

| Name | Returns | Summary |
| --- | --- | --- |
| Assign(UIItemInfo) | bool | Assigns the item. |
| Assign(UIItemInfo, Object) | bool | Assigns the item and passes the source to `onAssignWithSource`. |
| Assign(Object) | bool | Assigns from another item or equip slot. |
| Unassign() | void | Clears the slot. |
| GetItemInfo() | UIItemInfo | The assigned item, or null. |
| IsAssigned() | bool | Does the slot have an item. |
| AutoAssignID() | void | Sets the ID to the highest ID in the group plus one. |

## Static Methods

| Name | Returns | Summary |
| --- | --- | --- |
| GetSlots() | List&lt;UIItemSlot&gt; | All item slots active in the hierarchy. |
| GetSlotsWithID(int) | List&lt;UIItemSlot&gt; | Active slots with the ID. |
| GetSlotsInGroup(UIItemSlot_Group) | List&lt;UIItemSlot&gt; | Active slots in the group, sorted by ID. |
| GetSlot(int, UIItemSlot_Group) | UIItemSlot | The active slot with the ID and group, or null. |
| PrepareTooltip(UIItemInfo) | void | Adds the item lines to the [Tooltip](../tooltips/tooltip.md). |

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| slotGroup | UIItemSlot_Group | The slot group. |
| ID | int | The slot ID. |

## Code Usage
```
using UnityEngine;
using DuloGames.UI;

public class GiveItem : MonoBehaviour
{
    public void AddToInventory(int itemId)
    {
        UIItemInfo item = UIItemDatabase.Instance.GetByID(itemId);

        if (item == null)
            return;

        // Find the first free inventory slot
        foreach (UIItemSlot slot in UIItemSlot.GetSlotsInGroup(UIItemSlot_Group.Inventory))
        {
            if (!slot.IsAssigned())
            {
                slot.Assign(item);
                break;
            }
        }
    }

    public void AssignSlot(int id, UIItemSlot_Group group, int itemId)
    {
        UIItemSlot slot = UIItemSlot.GetSlot(id, group);

        if (slot == null || UIItemDatabase.Instance == null)
            return;

        slot.Assign(UIItemDatabase.Instance.GetByID(itemId));
    }
}
```

The built-in throw away confirm, for reference. Override `OnThrowAwayDenied()` in a subclass to change it.

```
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
        box.SetText1("Do you really want to destroy \"" + this.GetItemInfo().Name + "\"?");
        box.SetText2("You wont be able to reverse this operation and your item will be permamently removed.");
        box.SetConfirmButtonText("DESTROY");
        box.onConfirm.AddListener(Unassign);
        box.Show();
    }
}
```
