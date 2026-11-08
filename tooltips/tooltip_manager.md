# Tooltip Manager
A ScriptableObject that holds the [Tooltip](tooltip.md) prefab, the line styles and a few size settings. Every tooltip feature reads from it.

## Setup
1. Right click in the Project view and select **Create -> UI Managers -> Tooltip Manager**.
2. Save it as **TooltipManager** inside a **Resources** folder. It is loaded from **Resources/TooltipManager**; other names or folders are not found.
3. Assign your [Tooltip](tooltip.md) prefab to **Tooltip Prefab**.
4. Configure the **Default**, **Title** and **Description** line styles.
5. Add any **Custom Styles** you need and give each a unique **Name**.

Without this asset, `UITooltip.InstantiateIfNecessary` does nothing and no tooltip is shown.

## Properties

| Property | Summary |
| --- | --- |
| Tooltip Prefab | The [Tooltip](tooltip.md) prefab created in each canvas. |
| Spacer Height | Height of a spacer line, in pixels. |
| Item Tooltip Width | Width used by the [Item Slot](../icon_slots/item_slot.md) and [Equip Slot](../icon_slots/equip_slot.md) tooltips. |
| Spell Tooltip Width | Width used by the [Spell Slot](../icon_slots/spell_slot.md) tooltip. |
| Default Line Style | Used for lines without a style, and when a custom style name is not found. |
| Title Line Style | Used by `UITooltip.AddTitle`. |
| Description Line Style | Used by `UITooltip.AddDescription`. |
| Custom Styles | Named styles used by the `string customStyle` overloads. |

## Line Style

| Property | Summary |
| --- | --- |
| Name | Style name (custom styles only). |
| Font / Font Style / Font Size / Line Spacing | Text settings. **Font** is a TMP font asset; empty uses the default font. Line spacing is a multiplier (1 = normal). |
| Override Text Alignment | **No** aligns the left column left and the right column right. **Left**, **Center** or **Right** forces an alignment. |
| Font Color | Text color. |
| Text Effects | Shadow or Outline effects with color, distance and **Use Graphic Alpha**. One shadow and one outline are combined into a shared TMP material ([Text Utility](../miscellaneous/text_utility.md)). |

The built-in slot tooltips use these custom style names: **ItemAttribute**, **ItemStat**, **ItemDescription**, **SpellTitle**, **SpellAttribute** and **SpellDescription**. Keep them if you use the slots.

Use **Sort Custom Styles** from the component context menu to sort the custom styles by name.

## Methods

| Name | Returns | Summary |
| --- | --- | --- |
| GetCustomStyle(string) | UITooltipLineStyle | The custom style with the name, or the default style. |
| SortCustomStyles() | void | Sorts the custom styles by name. |

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| Instance (static) | UITooltipManager | The manager loaded from Resources/TooltipManager, or null. |
| prefab | GameObject | The tooltip prefab. |
| spacerHeight | int | The spacer height. |
| itemTooltipWidth | int | The item tooltip width. |
| spellTooltipWidth | int | The spell tooltip width. |
| defaultLineStyle | UITooltipLineStyle | The default style. |
| titleLineStyle | UITooltipLineStyle | The title style. |
| descriptionLineStyle | UITooltipLineStyle | The description style. |
| customStyles | UITooltipLineStyle[] | The custom styles. |

## Code Usage
```
using UnityEngine;
using DuloGames.UI;

public class TooltipCheck : MonoBehaviour
{
    void Start()
    {
        UITooltipManager manager = UITooltipManager.Instance;

        if (manager == null || manager.prefab == null)
        {
            Debug.LogWarning("Missing Resources/TooltipManager or its prefab.");
            return;
        }

        UITooltipLineStyle style = manager.GetCustomStyle("ItemStat");
        Debug.Log("ItemStat font size: " + style.TextFontSize);
    }
}
```
