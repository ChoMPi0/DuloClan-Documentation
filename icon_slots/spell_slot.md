# Spell Slot
A [Slot Base](slot_base.md) that holds a `UISpellInfo`. Use it for action bars and spell books. Slots are found by **Slot Group** and **Slot ID**, and can show a cooldown with [Slot Cooldown](slot_cooldown.md).

## Setup
1. Set up the slot object as described in [Slot Base](slot_base.md).
2. Add **Spell Slot** (Add Component -> UI -> Icon Slots -> Spell Slot).
3. Choose the **Slot Group** (None, Main_1 to Main_6, Extra, SpellBook, Talents).
4. Set a **Slot ID** that is unique in the group.
5. For spell books, turn on **Is Static** so dragging copies the spell instead of moving it.
6. To show cooldowns, add a [Slot Cooldown](slot_cooldown.md) and set this slot as its **Target Slot**.

In the editor, a slot with a duplicate ID in its group gets a new ID automatically when enabled, and a warning is logged. You can also use **Auto Assign ID** from the component context menu.

## Properties

| Property | Summary |
| --- | --- |
| Slot Group | The group used by `GetSlotsInGroup` and `GetSlot`. |
| Slot ID | The slot ID within its group. |

All other properties come from [Slot Base](slot_base.md).

## Behavior
- **Swapping:** swaps only with other spell slots.
- **Click:** `onClick` fires for any mouse button on an assigned slot. Check `eventData` in a subclass if you need a specific button.
- **Cooldown:** assigning a spell resumes its cooldown if one is running for that spell ID. Unassigning stops the cooldown display.
- **Tooltip:** shown after **Display Delay** and anchored to the slot. It uses **Spell Tooltip Width** from the manager and the custom styles **SpellTitle**, **SpellAttribute** and **SpellDescription**.

To change the tooltip content, edit `UISpellSlot.PrepareTooltip(UISpellInfo)`, or override `OnTooltip(bool)` in a subclass.

## Events
| Event | Summary |
| --- | --- |
| onAssign | The slot was assigned. |
| onUnassign | The slot was unassigned. Also called at the start of every assign. |
| onClick | An assigned slot was clicked. |

Eligible methods:

```
public void OnAssign(UISpellSlot slot) { }

public void OnUnassign(UISpellSlot slot) { }

public void OnClick(UISpellSlot slot) { }
```

## Methods

| Name | Returns | Summary |
| --- | --- | --- |
| Assign(UISpellInfo) | bool | Assigns the spell. |
| Assign(Object) | bool | Assigns from another spell slot. |
| Unassign() | void | Clears the slot. |
| GetSpellInfo() | UISpellInfo | The assigned spell, or null. |
| IsAssigned() | bool | Does the slot have a spell. |
| SetCooldownComponent(UISlotCooldown) | void | Links a cooldown component. Called by [Slot Cooldown](slot_cooldown.md) on Awake. |
| AutoAssignID() | void | Sets the ID to the highest ID in the group plus one. |

## Static Methods

| Name | Returns | Summary |
| --- | --- | --- |
| GetSlots() | List&lt;UISpellSlot&gt; | All spell slots active in the hierarchy. |
| GetSlotsWithID(int) | List&lt;UISpellSlot&gt; | Active slots with the ID. |
| GetSlotsInGroup(UISpellSlot_Group) | List&lt;UISpellSlot&gt; | Active slots in the group, sorted by ID. |
| GetSlot(int, UISpellSlot_Group) | UISpellSlot | The active slot with the ID and group, or null. |
| PrepareTooltip(UISpellInfo) | void | Adds the spell lines to the [Tooltip](../tooltips/tooltip.md). |

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| slotGroup | UISpellSlot_Group | The slot group. |
| ID | int | The slot ID. |
| cooldownComponent | UISlotCooldown | The linked cooldown component, or null (read only). |

## Code Usage
```
using UnityEngine;
using DuloGames.UI;

public class ActionBarSetup : MonoBehaviour
{
    public void AssignSlot(int slotId, UISpellSlot_Group group, int spellId)
    {
        UISpellSlot slot = UISpellSlot.GetSlot(slotId, group);

        if (slot == null || UISpellDatabase.Instance == null)
            return;

        slot.Assign(UISpellDatabase.Instance.GetByID(spellId));
        slot.onClick.AddListener(OnSpellClick);
    }

    private void OnSpellClick(UISpellSlot slot)
    {
        Debug.Log("Cast " + slot.GetSpellInfo().Name);
    }
}
```
