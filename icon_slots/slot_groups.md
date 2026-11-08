# Slot Groups
[Item Slots](item_slot.md) and [Spell Slots](spell_slot.md) can be placed in a **Slot Group**. Together with the **Slot ID**, the group lets you find a slot from code, for example "inventory slot 5" or "action bar 1, slot 3".

## Item Slot Groups (UIItemSlot_Group)

| Name | Value |
| --- | --- |
| None | 0 |
| Inventory | 1 |
| Dialog | 2 |
| QuestLog | 3 |
| Vendor | 4 |

## Spell Slot Groups (UISpellSlot_Group)

| Name | Value |
| --- | --- |
| None | 0 |
| Main_1 | 1 |
| Main_2 | 2 |
| Main_3 | 3 |
| Main_4 | 4 |
| Main_5 | 5 |
| Main_6 | 6 |
| Extra | 7 |
| SpellBook | 8 |
| Talents | 9 |

To add a group, add a new entry at the end of `UIItemSlot_Group.cs` or `UISpellSlot_Group.cs` with the next number. Do not change existing values; the group is saved as a number.

## Setup
1. Select the slot and choose the **Slot Group**.
2. Give it a **Slot ID** that is unique within the group. Right click the component header and use **Auto Assign ID** to pick the highest ID in the group plus one.

In the editor, when a slot is enabled with an ID that is already used in its group, a new ID is assigned and a warning is logged. This check is skipped in prefab mode.

## Static Methods
Both `UIItemSlot` and `UISpellSlot` have these methods. Only slots active in the hierarchy are returned.

| Name | Returns | Summary |
| --- | --- | --- |
| GetSlotsInGroup(group) | List | All slots in the group, sorted by ID. |
| GetSlot(int, group) | slot | The slot with the given ID in the group, or null. |

The [Equip Slot](equip_slot.md) uses the **Inventory** item group to return an item to the first free inventory slot.

## Code Usage
```
public void AddToInventory(UIItemInfo item)
{
    foreach (UIItemSlot slot in UIItemSlot.GetSlotsInGroup(UIItemSlot_Group.Inventory))
    {
        if (!slot.IsAssigned())
        {
            slot.Assign(item);
            return;
        }
    }
}
```
