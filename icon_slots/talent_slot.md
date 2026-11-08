# Talent Slot
A slot for talent trees. It shows a spell icon and a points label, and players spend points by clicking it. Talent slots extend [Slot Base](slot_base.md) but cannot be dragged.

## Setup
1. Add **Talent Slot** to a slot object and configure the icon like any [Slot Base](slot_base.md).
2. Assign the **Points Text** label.
3. Set the label colors: **Points Min Color** (no points), **Points Active Color** (some points) and **Points Max Color** (maxed).

## Usage
- **Left click** adds a point.
- **Right click** removes a point.

Points are clamped between 0 and the talent's `maxPoints`.

## Methods

| Name | Returns | Summary |
| --- | --- | --- |
| Assign(UITalentInfo, UISpellInfo) | bool | Assigns the talent and the spell used for the icon and tooltip. |
| GetTalentInfo() | UITalentInfo | The assigned talent. |
| GetSpellInfo() | UISpellInfo | The assigned spell. |
| AddPoints(int) | void | Adds (or with a negative value removes) points. |
| UpdatePointsLabel() | void | Refreshes the points label. |

## Code Usage
```
[SerializeField] private UITalentSlot m_Slot;

void Start()
{
    UITalentInfo talent = UITalentDatabase.Instance.GetByID(1);
    UISpellInfo spell = UISpellDatabase.Instance.GetByID(talent.spellEntry);

    this.m_Slot.Assign(talent, spell);
}
```
