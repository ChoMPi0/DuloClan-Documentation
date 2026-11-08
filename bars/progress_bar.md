# Progress Bar
A bar driven by a fill amount from 0 to 1. Use it for health, mana, experience or loading bars. The [Cast Bar](cast_bar.md) uses it for its fill.

## Setup
1. Add **Progress Bar** (UI/Bars/Progress Bar) to the bar object.
2. Choose the **Fill Type**.
3. Assign the **Fill Target** (and the settings for that type, see below).
4. Optionally set **Steps**.
5. Set the **Fill Amount**.

## Types

| Fill Type | Fill Target | Behavior |
| --- | --- | --- |
| Filled | Image of type Filled | Sets the image fill amount. |
| Resize | RectTransform | Changes the width of the target. Use it with a mask when the fill sprite should not stretch. |
| Sprites | Image, plus a **Sprites** array | Shows one sprite from the array based on the fill. At 0 fill the image is hidden. |

For Resize, **Fill Sizing** sets the width range:
- **Parent**: the width goes from 0 to the parent width. The target anchors are set to its pivot.
- **Fixed**: the width goes from **Min Width** to **Max Width**.

## Steps
**Steps** snaps the displayed fill to a fixed number of positions. 0 means no stepping. The first and last steps are empty and full, so 5 steps show 0, 0.25, 0.5, 0.75 and 1. Use 0 or a value of 2 or more.

`fillAmount` keeps the exact value. Only the display is snapped. Use `currentStep` to read or set the step (0 to Steps - 1).

## Events
`onChange` is a `UnityEvent<float>`. It passes the new fill amount and fires only when the value changes.

```
public void BarOnChange(float fillAmount)
{
    Debug.Log(fillAmount);
}
```

## Methods

| Name | Returns | Summary |
| --- | --- | --- |
| UpdateBarFill() | void | Re-applies the fill to the target. |
| AddFill() | void | Adds one step, or 0.1 without steps. Useful on buttons. |
| RemoveFill() | void | Removes one step, or 0.1 without steps. Useful on buttons. |

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| fillAmount | float | The fill amount, clamped to 0..1. Setting it updates the bar and fires onChange. |
| type | UIProgressBar.Type | Filled, Resize or Sprites. |
| targetImage | Image | The target image (Filled and Sprites). |
| targetTransform | RectTransform | The target transform (Resize). |
| sprites | Sprite[] | The sprites (Sprites). |
| minWidth | float | Minimum width for Resize with Fixed sizing. Setting it updates the bar. |
| maxWidth | float | Maximum width for Resize with Fixed sizing. Setting it updates the bar. |
| steps | int | The number of steps (0 for none). |
| currentStep | int | The current step. Setting it sets the fill amount (only when Steps is above 0). |

UIProgressBar implements `IUIProgressBar`, which exposes `fillAmount`.

## Code Usage
```
[SerializeField] private UIProgressBar m_HealthBar;

public void SetHealth(float current, float max)
{
    if (this.m_HealthBar == null || max <= 0f)
        return;

    this.m_HealthBar.fillAmount = current / max;
}
```
