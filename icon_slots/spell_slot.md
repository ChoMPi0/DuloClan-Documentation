# Spell Slot
An extension of [Slot Base](slot_base.md) designed for spells, in addition this slot can have an **ID** and **Slot Group**.

## Tooltip
In order to change the spell slot tooltip you have to locate and change the method **PrepareTooltip(UISpellInfo spellInfo)** inside the cs file.

## Cooldown
In order to to have cooldown you have to setup the component [Slot Cooldown](slot_cooldown.md).

## Events
The slot has three events, **onAssign**, **onUnassign** and **onClick**.

Here is an example of eligible methods for the events.

```
public void OnAssign(UISpellSlot slot) { }

public void OnUnassign(UISpellSlot slot) { }

public void OnClick(UISpellSlot slot) { }
```

## Code Usage
You can access any slot via the static methods **GetSlots()**, **GetSlotsWithID(int ID)**, **GetSlotsInGroup(UISpellSlot_Group group)** and **GetSlot(int ID, UISpellSlot_Group group)**.

Here is an example of how to get and assign a spell slot by id and group.

```
public void TestAssignSlot(int id, UISpellSlot_Group group)
{
    UISpellSlot slot = UISpellSlot.GetSlot(id, group);
    
    if (slot == null || UISpellDatabase.Instance == null)
        return;

    slot.Assign(UISpellDatabase.Instance.GetByID(1));
}
```
