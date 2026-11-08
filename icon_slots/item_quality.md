# Item Quality
Every item in the [Item Database](databases.md) has a **Quality**. The quality decides the color of the item name in the [Item Slot](item_slot.md) tooltip, and can tint a slot graphic with **UI Item Slot Set Color From Quality**.

## Qualities

| Quality | Value | Hex color |
| --- | --- | --- |
| Poor | 0 | `9d9d9dff` |
| Common | 1 | `ffffffff` |
| Uncommon | 2 | `1eff00ff` |
| Rare | 3 | `0070ffff` |
| Epic | 4 | `a335eeff` |
| Legendary | 5 | `ff8000ff` |

The colors are constants in the `UIItemQualityColor` class. Change them there to restyle every quality.

## Set Color From Quality
Tints a graphic (for example a border or glow) with the quality color of the item in the slot.

1. Add **UI Item Slot Set Color From Quality** to an object with an **Item Slot**.
2. Assign the **Target** graphic. It is required.

When an item is assigned, the **Target** color is set to the item's quality color. When the slot is unassigned, the color is set to fully transparent. The color is only changed on assign and unassign, so set a starting color (usually transparent) on the target yourself.

## Static Methods (UIItemQualityColor)

| Name | Returns | Summary |
| --- | --- | --- |
| GetHexColor(UIItemQuality) | string | The hex color (RRGGBBAA) of the quality. |
| GetColor(UIItemQuality) | Color | The color of the quality. |

## Code Usage
```
public void ShowItemName(UIItemInfo item, Text label)
{
    label.text = "<color=#" + UIItemQualityColor.GetHexColor(item.Quality) + ">" + item.Name + "</color>";
}
```
