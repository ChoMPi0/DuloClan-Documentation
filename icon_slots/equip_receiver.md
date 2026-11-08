# Equip Receiver
A drop area for a character panel. When an [Item Slot](item_slot.md) is dropped on it, the item is forwarded to the matching [Equip Slot](equip_slot.md). Players can drop gear anywhere on the panel instead of on the exact slot.

## Setup
1. Add **Equip Receiver** (Add Component -> UI -> Icon Slots -> Equip Receiver) to an object with a raycast target graphic that covers the panel.
2. Assign **Slots Container** to the transform that holds the equip slots. When empty, the receiver's own transform is used.
3. Optionally assign a **Hint Text** to show while dragging over the receiver.
4. Set **Fading** and **Fade Duration** for the hint, and its starting **Hint State**.

## Properties

| Property | Summary |
| --- | --- |
| Slots Container | Parent of the equip slots to search. |
| Hint Text | Optional text shown while a slot is dragged over the receiver. |
| Fading | Fade the hint in and out instead of switching instantly. |
| Fade Duration | Hint fade duration in seconds. |
| Hint State | Starting state of the hint (Shown or Hidden). |

## Behavior
- On drop, the receiver reads the item's **EquipType** and finds the first enabled, active [Equip Slot](equip_slot.md) of that type under **Slots Container**. The drop is then passed to that slot, so swapping and type rules apply as usual.
- A drop on the receiver never counts as a throw away, even when no slot matches. The item stays in its source slot.
- Only item slots are forwarded. Other slots dropped here are ignored.
- The hint shows when any slot is dragged over the receiver and hides on exit or after a forwarded drop.

## Methods

| Name | Returns | Summary |
| --- | --- | --- |
| GetSlotByType(UIEquipmentType) | UIEquipSlot | The first enabled, active equip slot of the type under the container, or null. |

## Code Usage
```
using UnityEngine;
using DuloGames.UI;

public class AutoEquip : MonoBehaviour
{
    [SerializeField] private UIEquipReceiver m_Receiver;

    // Hook to UIItemSlot.onDoubleClick
    public void OnItemDoubleClick(UIItemSlot itemSlot)
    {
        UIEquipSlot equipSlot = this.m_Receiver.GetSlotByType(itemSlot.GetItemInfo().EquipType);

        if (equipSlot == null)
            return;

        if (!equipSlot.IsAssigned())
        {
            // Move the item into the empty equip slot
            if (equipSlot.Assign(itemSlot))
                itemSlot.Unassign();
        }
        else if (itemSlot.CanSwapWith(equipSlot))
        {
            // Swap with the equipped item
            itemSlot.PerformSlotSwap(equipSlot);
        }
    }
}
```
