# Color Schemes
Color schemes recolor the whole interface from one asset. Each colored graphic carries a **Color Scheme Element** that says which shade it uses; applying a scheme sets every element to that shade's color.

## Create a Color Scheme
Right click in the Project view and select **Create -> UI Color Scheme**, then set the colors in the inspector.

## Apply a Color Scheme
Select the color scheme asset and press **Apply**. This recolors every element in the open scenes (including inactive objects such as closed windows), records the change for **Undo** and as prefab instance overrides, and makes the scheme the active one.

New elements added later pick up the active scheme automatically.

## Color Scheme Manager
The **ColorSchemeManager** asset stores the active scheme. It must be in a **Resources** folder (Create -> UI Managers -> Color Scheme Manager).

## Color Scheme Element
Add **UI/Color Scheme Element** to any object with a Graphic (Image, Text, ...) and choose a **Shade**. Only the RGB is changed; the graphic's alpha is kept.

For [Select Fields](../controls/select_field.md) use **Color Scheme Element (Select Field)** and choose whether it colors the list background or the separators.

## Shades

| Shade | Color scheme property |
| --- | --- |
| Primary | imagePrimary |
| Secondary | imageSecondary |
| Light | imageLight |
| Dark | imageDark |
| Effect, Effect2, Effect3 | imageEffect, imageEffect2, imageEffect3 |
| BordersPrimary, BordersSecondary | imageBordersPrimary, imageBordersSecondary |
| ButtonPrimary | buttonForeground |
| ButtonSecondary | buttonEffect |
| WindowHeader | windowHeader |
| Animation | animation |

## Code Usage
```
[SerializeField] private ColorScheme m_NightScheme;

public void UseNightScheme()
{
    // Recolors all elements and sets the scheme as active
    this.m_NightScheme.ApplyColorScheme();
}

public Color GetPrimaryColor()
{
    ColorScheme active = ColorSchemeManager.Instance.activeColorScheme;
    return active.GetColorShade(ColorSchemeShade.Primary);
}
```

Setting `ColorSchemeManager.Instance.activeColorScheme` in the editor saves it to the asset, also in play mode.

## Custom Elements
Implement `IColorSchemeElement` (a `shade` property and an `Apply(Color)` method) and apply it with `ColorScheme.ApplyToElement(element)`.
