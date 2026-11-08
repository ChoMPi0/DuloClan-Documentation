# Databases
Simple ScriptableObject databases for items, spells and talents. They are used by the demo scenes and are a quick way to feed the [Icon Slots](slot_base.md). For a real game you will usually load this data from your own systems and only pass the info objects to the slots.

## Create a Database
Right click in the Project view and select **Create -> Databases -> Item Database** (or Spell / Talent Database).

The `Instance` properties load the databases from a **Resources** folder by these paths:

| Database | Resources path |
| --- | --- |
| UIItemDatabase | Databases/ItemDatabase |
| UISpellDatabase | Databases/SpellDatabase |
| UITalentDatabase | Databases/TalentDatabase |

## Methods
Each database has the same two methods.

| Name | Returns | Summary |
| --- | --- | --- |
| Get(int index) | info | Gets the entry at the array index. |
| GetByID(int id) | info | Gets the entry with the given ID, or null. |

## Info Classes

| Class | Fields |
| --- | --- |
| UIItemInfo | ID, Name, Icon, Description, Quality, EquipType, ItemType, Type, Subtype, Damage, AttackSpeed, Block, Armor, Stamina, Strength, Durability, RequiredLevel |
| UISpellInfo | ID, Name, Icon, Description, Range, Cooldown, CastTime, PowerCost, Flags |
| UITalentInfo | ID, spellEntry (spell ID), maxPoints |

## Code Usage
```
[SerializeField] private UIItemSlot m_Slot;

void Start()
{
    UIItemInfo sword = UIItemDatabase.Instance.GetByID(12);

    if (sword != null)
        this.m_Slot.Assign(sword);
}
```
