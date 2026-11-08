# Equip Slot
A [Slot Base](slot_base.md) for character equipment. It holds a `UIItemInfo` and only accepts items whose **EquipType** matches the slot's **Equip Type**.

## Setup
1. Set up the slot object as described in [Slot Base](slot_base.md).
2. Add **Equip Slot** (Add Component -> UI -> Icon Slots -> Equip Slot).
3. Set the **Equip Type** (Head, Chest, Weapon_MainHand, ...).
4. Optionally add an [Equip Receiver](equip_receiver.md) over the character panel, so items dropped anywhere on it go to the right slot.

## Properties

| Property | Summary |
| --- | --- |
| Equip Type | The `UIEquipmentType` this slot accepts. |

All other properties come from [Slot Base](slot_base.md).

## Behavior
- **Type check:** every assign checks the equip type. Items of another type are refused, from code and from drag and drop.
- **Swapping:** swaps with item slots and other equip slots when the types match.
- **Throw away:** when **Allow Throw Away** is off, dropping the item outside a slot moves it to the first free [Item Slot](item_slot.md) in the **Inventory** group, then clears the equip slot. Nothing happens if there is no free slot.
- **Tooltip:** an empty slot shows its type name (for example "Main Hand"). An assigned slot shows the item tooltip from `UIItemSlot.PrepareTooltip(UIItemInfo)` in [Item Slot](item_slot.md).

## Events
| Event | Summary |
| --- | --- |
| onAssign | The slot was assigned. |
| onAssignWithSource | The slot was assigned. Also passes the source slot (null when assigned from code). |
| onUnassign | The slot was unassigned. Also called at the start of every assign. |

Eligible methods:

```
public void OnAssign(UIEquipSlot slot) { }

public void OnAssignWithSource(UIEquipSlot slot, Object source) { }

public void OnUnassign(UIEquipSlot slot) { }
```

## Methods

| Name | Returns | Summary |
| --- | --- | --- |
| Assign(UIItemInfo) | bool | Assigns the item if the type matches. |
| Assign(UIItemInfo, Object) | bool | Same, and passes the source to `onAssignWithSource`. |
| Assign(Object) | bool | Assigns from another item or equip slot if the type matches. |
| Unassign() | void | Clears the slot. |
| GetItemInfo() | UIItemInfo | The assigned item, or null. |
| IsAssigned() | bool | Does the slot have an item. |
| CheckEquipType(UIItemInfo) | bool | Does the item fit this slot. Virtual, override it for custom rules. |

## Static Methods

| Name | Returns | Summary |
| --- | --- | --- |
| GetSlots() | List&lt;UIEquipSlot&gt; | All equip slots active in the hierarchy. |
| GetSlotWithType(UIEquipmentType) | UIEquipSlot | The first active slot of the type, or null. |
| GetSlotsWithType(UIEquipmentType) | List&lt;UIEquipSlot&gt; | All active slots of the type (for example two rings). |
| EquipTypeToString(UIEquipmentType) | string | A display name for the type. |

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| equipType | UIEquipmentType | The equip type. |

## Code Usage
```
using UnityEngine;
using DuloGames.UI;

public class EquipItem : MonoBehaviour
{
    public bool Equip(int itemId)
    {
        UIItemInfo item = UIItemDatabase.Instance.GetByID(itemId);

        if (item == null)
            return false;

        UIEquipSlot slot = UIEquipSlot.GetSlotWithType(item.EquipType);

        if (slot == null)
            return false;

        return slot.Assign(item);
    }
}
```

The built-in throw away handler, for reference. Override `OnThrowAwayDenied()` in a subclass to change it.

```
protected override void OnThrowAwayDenied()
{
    if (!this.IsAssigned())
        return;

    // Find free inventory slot
    List<UIItemSlot> itemSlots = UIItemSlot.GetSlotsInGroup(UIItemSlot_Group.Inventory);

    foreach (UIItemSlot slot in itemSlots)
    {
        if (!slot.IsAssigned())
        {
            // Move the item to the inventory slot
            slot.Assign(this);

            // Clear this equip slot
            this.Unassign();
            break;
        }
    }
}
```
