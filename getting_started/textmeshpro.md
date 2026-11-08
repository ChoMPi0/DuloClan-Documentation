# TextMeshPro
Since scripts version 2.0 every text in the UI is a **TextMeshPro** text (`TMP_Text` / `TextMeshProUGUI`), input fields are **TMP_InputField** and fonts are **TMP Font Assets**. Legacy **Text**, **InputField** and **Font** are no longer used by the scripts.

## Setup
TextMeshPro ships with the Unity UI package in Unity 6. Before opening the demo scenes, import its resources once:

**Window -> TextMeshPro -> Import TMP Essential Resources**

This creates `Assets/TextMesh Pro` with the **TMP Settings** asset and the default font. Without it, texts have no font and the console shows *"TextMesh Pro Essential Resources are missing"*.

## Fonts
Each font used by the asset has a TMP font asset next to it (`<Font Name> SDF.asset`). They are **Dynamic**: missing characters are added to the atlas at runtime, so any language works with the font's own glyphs.

Font fields on the components ([Select Field](../controls/select_field.md), [Slider Extended](../controls/slider_extended.md), [Tooltip Manager](../tooltips/tooltip_manager.md) line styles) take a TMP font asset. Leave them empty to use the default font from **TMP Settings**.

To use your own font, select the font file and use **Window -> TextMeshPro -> Font Asset Creator**, or right click it and select **Create -> TextMeshPro -> Font Asset**.

## Shadows and Outlines
Unity's **Shadow** and **Outline** components (and [Nicer Outline](../effects/nicer_outline.md)) do not work on TextMeshPro texts. Text shadows and outlines are part of the TMP **material** instead (Underlay and Outline):
- The converted prefabs use material presets stored in the font's `Presets` folder. Pick one in the text's **Material Preset** field.
- To make a new preset, duplicate one, then change its **Underlay** or **Outline** settings.
- Components that style texts at runtime (select field options, slider options, tooltip lines) still expose **Shadow / Outline** settings. They build a matching material for you (see [Text Utility](../miscellaneous/text_utility.md)).

Spacing and gradients are also built in: use **Character Spacing** and **Color Gradient** on the TMP text instead of [Letter Spacing](../effects/letter_spacing.md) and [Gradient](../effects/gradient.md).

## Converting Your Own Prefabs
Updating from scripts 1.x changes the type of every text field (for example `Text m_LabelText` is now `TMP_Text m_LabelText`). Prefabs and scenes you made yourself still contain legacy Text components, so their references show as empty and the console logs *"Serialized reference type mismatch"* warnings.

The **TextMeshPro Converter** fixes this. It rewrites prefabs, scenes and assets in place, replacing each component in its original slot, so every reference to it (in the same prefab, from other prefabs and from scenes) stays assigned.

> **Back up your project or commit it to version control first.** The conversion rewrites files on disk and cannot be undone.

1. Import the TMP Essential Resources (see [Setup](#setup)).
2. Update the scripts. **Do not open and save** any of your scenes or prefabs before step 4: saving them now would store the mismatched references as empty.
3. Select **Tools -> DuloGames -> TextMeshPro Converter -> Analyze Folder (Dry Run)...** and pick the folder with your UI. Nothing is written; the report shows what would change.
4. Select **Tools -> DuloGames -> TextMeshPro Converter -> Convert Folder...** and pick the same folder.
5. Open your scenes and check the texts.

The report is printed to the console and saved to `Logs/TMPConversionReport.txt` in the project folder.

### What It Converts

| Legacy | Converted to |
| --- | --- |
| Text | TextMeshPro - Text (UI). Text, color, size, style, alignment, wrapping, line spacing, rich text and raycast target are kept. **Best Fit** becomes **Auto Size**. |
| Input Field | TMP Input Field. The parent of its text (or the field itself) becomes the text viewport. Content type, character validation, character limit, colors and events are kept. |
| Shadow, Outline, Nicer Outline on a text | Removed and replaced by a material preset with the same color and offset. |
| Font | A dynamic TMP font asset created next to the font (`<Font Name> SDF.asset`). |
| Font fields on DuloGames components | The matching TMP font asset. |
| Prefab overrides on texts | Remapped to the TMP properties. |

### Limits
- Shadow and outline presets approximate the legacy pixel offsets; thick or far-offset effects can look slightly different. Adjust the preset material if needed.
- Custom materials on legacy texts are not compatible with TMP and are removed. The report lists them.
- A Shadow or Outline **added** to a nested prefab instance (an added component override) is not converted. The report lists it; add a material preset by hand.
- Fonts that can't be found fall back to the TMP default font. The report lists them.
- Your own scripts that use `Text`, `InputField` or `Font` must be changed to `TMP_Text`, `TMP_InputField` and `TMP_FontAsset` (namespace `TMPro`) before converting.
