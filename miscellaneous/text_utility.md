# Text Utility
`UITextUtility` is a static class that applies legacy-style text settings to **TextMeshPro** texts: fonts with a default fallback, alignment, font style, wrapping, line spacing, best fit, and Shadow / Outline effects. The [Select Field](../controls/select_field.md), [Slider Extended](../controls/slider_extended.md) and [Tooltip](../tooltips/tooltip.md) use it to style the texts they create at runtime.

## Effects
TextMeshPro has no Shadow or Outline components; both are material features (Underlay and Outline). `ApplyEffect` and `ApplyEffects` build a material from the text's font material with the requested effect and assign it as the text's shared material.

- Materials are cached and shared: texts with the same font and the same effect use one material, so batching is kept.
- The effect **Distance** is in pixels, like Unity's Shadow and Outline, and is converted relative to the font size. One shadow and one outline can be combined; with several of the same type, the last one wins.
- TMP outlines are centered on the glyph edge; the face is dilated by the same amount so the whole outline sits outside the letters, like a legacy Outline.
- `UITextEffectType.None` (or an empty list) restores the font's default material.
- The created materials are runtime only and are not saved. For texts in your prefabs, use a material preset instead (see [TextMeshPro](../getting_started/textmeshpro.md#shadows-and-outlines)).
- **Use Graphic Alpha** is kept for compatibility; TextMeshPro always fades the effect with the text alpha.

## Static Methods

| Name | Returns | Summary |
| --- | --- | --- |
| GetFontOrDefault(TMP_FontAsset) | TMP_FontAsset | The font, or the default font when null. |
| ApplyFont(TMP_Text, TMP_FontAsset) | void | Assigns the font, or the default font when null. |
| ApplyWrapModes(TMP_Text, HorizontalWrapMode, VerticalWrapMode) | void | Applies legacy wrap modes. **Wrap** wraps, **Overflow** doesn't; **Truncate** truncates. |
| ApplyBestFit(TMP_Text, bool, int, int) | void | Applies legacy best fit as TMP auto size with min and max size. |
| ApplyEffect(TMP_Text, UITextEffectType, Color, Vector2, bool) | void | Applies one Shadow or Outline effect (color, distance, use graphic alpha). |
| ApplyEffects(TMP_Text, IList&lt;UITextEffect&gt;) | void | Applies a shadow and / or an outline. |
| SetupEffectMaterial(Material, TMP_FontAsset, float, bool, Color, Vector2, bool, Color, float) | void | Configures a copy of a font material to match legacy Shadow / Outline components on a text of the given font size: shadow on / color / distance, outline on / color / thickness, in pixels. Use it to build your own material presets. |
| ToTMPAlignment(TextAnchor) | TextAlignmentOptions | Converts a legacy text anchor. |
| ToTMPStyle(FontStyle) | FontStyles | Converts a legacy font style. |
| ToTMPWrapping(HorizontalWrapMode) | TextWrappingModes | Converts a legacy horizontal wrap mode. |
| ToTMPOverflow(VerticalWrapMode) | TextOverflowModes | Converts a legacy vertical wrap mode. |
| ToTMPLineSpacing(float) | float | Converts a legacy line spacing multiplier (1 = normal) to TMP line spacing (0 = normal). |
| ClearStaticCache() | void | Clears the material cache. Called automatically when entering play mode. |

## Static Properties

| Name | Type | Summary |
| --- | --- | --- |
| DefaultFont | TMP_FontAsset | The default font from **TMP Settings**, or null if the TMP Essential Resources are not imported. |

## Types

| Name | Summary |
| --- | --- |
| UITextEffectType | **None**, **Shadow** or **Outline**. |
| UITextEffect | A serializable effect: `type`, `color`, `distance` and `useGraphicAlpha`. |

## Code Usage
Create a label at runtime with the asset's font and a shadow.

```
using TMPro;
using UnityEngine;
using DuloGames.UI;

public class LabelFactory : MonoBehaviour
{
    [SerializeField] private TMP_FontAsset m_Font;

    public TMP_Text CreateLabel(Transform parent, string value)
    {
        GameObject go = new GameObject("Label", typeof(RectTransform));
        go.transform.SetParent(parent, false);

        TextMeshProUGUI text = go.AddComponent<TextMeshProUGUI>();
        UITextUtility.ApplyFont(text, this.m_Font);
        text.fontSize = 24;
        text.fontStyle = UITextUtility.ToTMPStyle(FontStyle.Bold);
        text.alignment = UITextUtility.ToTMPAlignment(TextAnchor.MiddleCenter);
        text.text = value;

        // Black shadow, 1 pixel right and 1 pixel down
        UITextUtility.ApplyEffect(text, UITextEffectType.Shadow, new Color(0f, 0f, 0f, 0.5f), new Vector2(1f, -1f), true);

        return text;
    }
}
```
