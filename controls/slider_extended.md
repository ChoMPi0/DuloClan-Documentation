# Slider Extended
A Unity Slider with optional labeled options (for example Low, Medium, High). Each option is a step on the slider. Use it for settings with a few fixed levels.

## Setup
1. Add **Slider Extended** (UI/Slider Extended) instead of a regular Slider and set it up like a Slider (Fill Rect, Handle Rect, ...).
2. Enter the options in the **Options** text area, one per line.
3. Assign the option **Sprite** under **Option Image Properties**. Its size is used as the grid cell size.
4. Style the option text and transition.

When the slider has options it creates an "Options Grid" child and one object per option, spread evenly across the slider width. It also forces **Whole Numbers** on, **Min Value** to 0 and **Max Value** to the option count minus one. The slider value is the selected option index.

Leave **Options** empty to use it as a regular Slider.

## Inspector Settings

| Property | Summary |
| --- | --- |
| **Options** | One option per line. |
| **Options Padding** | Padding of the options grid. |
| **Sprite** | The option image sprite. |
| **Text Font**, **Text Font Style**, **Text Font Size**, **Text Color** | Option text style. **Text Font** is a TMP font asset; empty uses the default font. |
| **Text Effect** | None, Shadow or Outline, with **Color**, **Distance** and **Use graphic alpha**. Applied as a shared TMP material ([Text Utility](../miscellaneous/text_utility.md)). |
| **Text Offset** | Position offset of the option text. |
| **Text Transition** | None or ColorTint for the selected option. |
| **Target** | Image or Text: what the color tint applies to. |
| **Color Normal**, **Color Active** | Tint of unselected and selected options. |
| **Color Multiplier** | Multiplies the tint color (1 to 6). |
| **Duration** | Tint fade duration in seconds. |

## Events
`onValueChanged` is the standard Slider `UnityEvent<float>`. With options, the value is the option index.

```
public void OnValueChanged(float value)
{
    Debug.Log("Slider value: " + value);
}
```

## Methods

| Name | Returns | Summary |
| --- | --- | --- |
| HasOptions() | bool | Does the slider have options. |
| UpdateGridProperties() | void | Re-applies the grid padding, cell size and spacing. |
| UpdateOptionsProperties() | void | Re-applies the option sprite, text style and colors. |
| RebuildTextEffects() | void | Removes and re-adds the option text effects. |

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| options | List&lt;string&gt; | The options. Setting it rebuilds the option objects and re-applies whole numbers and min/max. |
| selectedOptionIndex | int | The index of the selected option (0 if out of range). |
| selectedOptionGameObject | GameObject | The selected option object. |
| optionsPadding | RectOffset | The options grid padding. |

Assign a new list to `options` to change the options from code. Editing the returned list in place does not rebuild the option objects.

## Code Usage
Fill the slider with the quality levels and select the current one.

```
using System.Collections.Generic;
using UnityEngine;
using DuloGames.UI;

public class QualitySlider : MonoBehaviour
{
    [SerializeField] private UISliderExtended m_Slider;

    private void Start()
    {
        this.m_Slider.options = new List<string>(QualitySettings.names);
        this.m_Slider.value = QualitySettings.GetQualityLevel();
        this.m_Slider.onValueChanged.AddListener(OnValueChanged);
    }

    private void OnValueChanged(float value)
    {
        QualitySettings.SetQualityLevel(this.m_Slider.selectedOptionIndex);
    }
}
```
