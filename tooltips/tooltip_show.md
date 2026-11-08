# Tooltip Show
Shows a [Tooltip](tooltip.md) with fixed lines when the pointer hovers the object. Use it for buttons, icons and labels that need a tooltip without code.

## Setup
1. Make sure the [Tooltip Manager](tooltip_manager.md) exists at **Resources/TooltipManager** and has a tooltip prefab.
2. Add **Tooltip Show** (Add Component -> UI -> Tooltip Show) to an object with a raycast target graphic.
3. Add **Content Lines** and fill them in.
4. Set **Position**, **Width Mode** and **Delay**.

## Properties

| Property | Summary |
| --- | --- |
| Position | **Floating** places the tooltip at the pointer. **Anchored** anchors it to this object. |
| Width Mode | **Default** uses the tooltip's **Default Width**. **Preferred** sizes the width to the content. |
| Override Offset | Replaces the tooltip offsets with **Offset** for this display. |
| Offset | The offset used when **Override Offset** is on. It is applied to both the pointer and the anchored offset. |
| Delay | Seconds to wait after the pointer enters before showing (0-10). 0 shows instantly. |
| Content Lines | The lines to show, in order. |

Each content line has:

| Field | Summary |
| --- | --- |
| Style | **Default**, **Title**, **Description** or **Custom**. |
| Style Name | The custom style name from the manager (for **Custom**). |
| Content | The line text. Rich text is supported. |
| Is Spacer ? | Adds a spacer line instead of text. |

## Behavior
- The tooltip is created in this object's canvas when the pointer enters.
- It hides when the pointer exits or is pressed.
- Nothing happens while the component is disabled.

## Methods

| Name | Returns | Summary |
| --- | --- | --- |
| OnTooltip(bool) | void | Builds and shows (true) or hides (false) the tooltip. Virtual, override it to add lines from code. |

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| contentLines | UITooltipLineContent[] | Gets or sets the content lines. |

## Code Usage
```
using UnityEngine;
using DuloGames.UI;

public class SetTooltipText : MonoBehaviour
{
    [SerializeField] private UITooltipShow m_TooltipShow;

    void Start()
    {
        UITooltipLineContent title = new UITooltipLineContent();
        title.LineStyle = UITooltipLines.LineStyle.Title;
        title.Content = "Options";

        UITooltipLineContent text = new UITooltipLineContent();
        text.Content = "Opens the options window.";

        this.m_TooltipShow.contentLines = new UITooltipLineContent[] { title, text };
    }
}
```
