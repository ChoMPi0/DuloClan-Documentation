# Tooltip
A single shared tooltip that builds its text lines at runtime. Slots, [Tooltip Show](tooltip_show.md) and your own scripts fill it through static methods, then show and hide it.

## Setup
1. Create an object under your canvas and add **Tooltip** (Add Component -> UI -> Tooltip). A **Canvas Group**, **Vertical Layout Group** and **Content Size Fitter** are added automatically.
2. Add a background image and set the padding on the **Vertical Layout Group**.
3. Set the properties below.
4. Make it a prefab and assign it to **Tooltip Prefab** on the [Tooltip Manager](tooltip_manager.md).
5. Configure the line styles on the [Tooltip Manager](tooltip_manager.md).
6. Show it with [Tooltip Show](tooltip_show.md), a slot, or from code.

The tooltip only works when the [Tooltip Manager](tooltip_manager.md) asset exists at **Resources/TooltipManager**. Line styles and the spacer height are read from it.

## Properties

| Property | Summary |
| --- | --- |
| Follow Mouse | Keeps moving the tooltip with the mouse while it is visible. |
| Offset | Offset from the pointer when the tooltip is not anchored to a rect. |
| Anchored Offset | Offset from the target rect when anchored with `AnchorToRect`. |
| Default Width | Width used when no width is set for the current display. |
| Anchoring | **Corners**, **Left Or Right** or **Top Or Bottom**. The side is picked from the screen half the mouse is in. |
| Transition | **None** or **Fade**. |
| Easing / Duration | Fade tween settings (shown for **Fade**). |
| Graphic (Anchor Graphic) | Optional arrow graphic. It is moved and flipped to match the current pivot. |
| Offset (Anchor Graphic) | Offset of the anchor graphic. |

## Behavior
- Only one tooltip exists at a time. `UITooltip.Instance` is the active one.
- `InstantiateIfNecessary(go)` creates the prefab in the canvas of `go`. If the current tooltip is in a different canvas, it is destroyed and a new one is created. A tooltip placed in the scene by hand is used when it is in the same canvas.
- Lines are built when `Show()` is called. Call the `Add...` methods first.
- When the tooltip hides, the lines are destroyed. The width, horizontal fit mode, offsets and anchor target are reset. Children with a **Layout Element** set to **Ignore Layout** are kept, so you can use them for decoration.
- The tooltip never blocks raycasts. An [Always On Top](../miscellaneous/always_on_top.md) component is added to keep it above other UI.
- The tooltip itself has no show delay. The delay comes from the caller, for example **Delay** on [Tooltip Show](tooltip_show.md) or **Display Delay** on a [slot](../icon_slots/slot_base.md).

## Lines
Each line has a left and a right column. Single lines use the left column only. Every line has a style: **Default**, **Title**, **Description** or **Custom** (a named custom style from the manager). An unknown custom style name falls back to the default style.

`AddLineColumn` adds a left column to a new line, and the next call fills the right column of that line. The style of a line comes from its first column.

**Note:** the two-string `AddLine` overloads do different things on the two classes.

| Call | Meaning |
| --- | --- |
| `UITooltip.AddLine(string content, string customStyle)` | One line. The second string is a custom style name. |
| `UITooltipLines.AddLine(string left, string right)` | One line with a left and a right column. |

For two columns on `UITooltip`, call `AddLineColumn` twice.

## Events
`onAnchorEvent` passes the new `UITooltip.Anchor` when the pivot changes. It is only invoked when an anchor graphic is assigned, and it is not shown in the inspector. Add listeners from code.

```
void Start()
{
    if (UITooltip.Instance != null)
        UITooltip.Instance.onAnchorEvent.AddListener(OnTooltipAnchor);
}

public void OnTooltipAnchor(UITooltip.Anchor anchor)
{
    Debug.Log("Tooltip anchored " + anchor);
}
```

## Static Methods

| Name | Returns | Summary |
| --- | --- | --- |
| InstantiateIfNecessary(GameObject) | void | Creates the tooltip prefab in the canvas of the object, if needed. Call it first. |
| AddTitle(string) | void | Adds a line with the title style. |
| AddDescription(string) | void | Adds a line with the description style. |
| AddLine(string) | void | Adds a line with the default style. |
| AddLine(string, UITooltipLines.LineStyle) | void | Adds a line with a style. |
| AddLine(string, string) | void | Adds a line with a named custom style. Not two columns. |
| AddLine(string, RectOffset) | void | Adds a line with padding. |
| AddLine(string, RectOffset, UITooltipLines.LineStyle) | void | Adds a line with padding and a style. |
| AddLine(string, RectOffset, string) | void | Adds a line with padding and a named custom style. |
| AddLineColumn(string) | void | Adds a column to the last incomplete line, or starts a new line. |
| AddLineColumn(string, UITooltipLines.LineStyle) | void | Same, with a style for a new line. |
| AddLineColumn(string, string) | void | Same, with a named custom style for a new line. |
| AddSpacer() | void | Adds an empty line. Its height is **Spacer Height** on the manager. |
| SetLines(UITooltipLines) | void | Replaces the current lines with a prepared set. |
| SetWidth(float) | void | Sets the width for this display. |
| SetHorizontalFitMode(ContentSizeFitter.FitMode) | void | Sets the horizontal fit for this display (for example PreferredSize). |
| AnchorToRect(RectTransform) | void | Anchors the tooltip to a rect instead of the pointer. |
| OverrideOffset(Vector2) | void | Overrides **Offset** for this display. |
| OverrideAnchoredOffset(Vector2) | void | Overrides **Anchored Offset** for this display. |
| Show() | void | Builds the lines and shows the tooltip. |
| Hide() | void | Hides the tooltip. |
| VectorPivotToCorner(Vector2) | UITooltip.Corner | Converts a pivot to a corner. |
| VectorPivotToAnchor(Vector2) | UITooltip.Anchor | Converts a pivot to an anchor. |
| GetOppositeCorner(UITooltip.Corner) | UITooltip.Corner | Returns the opposite corner. |
| GetOppositeAnchor(UITooltip.Anchor) | UITooltip.Anchor | Returns the opposite anchor. |

All static methods do nothing when there is no tooltip instance.

## Object Oriented Methods

| Name | Returns | Summary |
| --- | --- | --- |
| IsActive() | bool | Is the component enabled and active in the hierarchy. |
| UpdatePositionAndPivot() | void | Repositions the tooltip. |
| UpdatePivot() | void | Picks the pivot from the mouse position. |
| SetAlpha(float) | void | Sets the canvas group alpha. |
| StartAlphaTween(float, float) | void | Tweens the alpha to a target over a duration. |

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| Instance (static) | UITooltip | The active tooltip, or null. |
| defaultWidth | float | The default width. |
| followMouse | bool | Follow the mouse. |
| offset | Vector2 | Offset from the pointer. |
| anchoredOffset | Vector2 | Offset from the anchored rect. |
| anchoring | UITooltip.Anchoring | The anchoring mode. |
| transition | UITooltip.Transition | The transition. |
| transitionEasing | TweenEasing | The fade easing. |
| transitionDuration | float | The fade duration. |
| alpha | float | Current alpha (read only). |
| visualState | UITooltip.VisualState | Shown or Hidden (read only). |
| uiCamera | Camera | Camera of the tooltip canvas, or null for overlay canvases (read only). |

## Code Usage
```
using UnityEngine;
using UnityEngine.UI;
using DuloGames.UI;

public class MyTooltip : MonoBehaviour
{
    public void OnTooltip(bool show)
    {
        if (UITooltipManager.Instance == null)
            return;

        if (show)
        {
            UITooltip.InstantiateIfNecessary(this.gameObject);

            UITooltip.AddTitle("Tooltip Title");
            UITooltip.AddSpacer();
            UITooltip.AddLine("A line of text");

            // Two columns on one line
            UITooltip.AddLineColumn("Left column text");
            UITooltip.AddLineColumn("Right column text");

            // A line with a custom style defined in the manager
            UITooltip.AddLine("Styled line", "ItemAttribute");

            UITooltip.AddDescription("Some very simple description");

            UITooltip.SetWidth(512f);
            UITooltip.SetHorizontalFitMode(ContentSizeFitter.FitMode.PreferredSize);
            UITooltip.AnchorToRect(this.transform as RectTransform);
            UITooltip.OverrideAnchoredOffset(Vector2.zero);

            UITooltip.Show();
        }
        else
        {
            UITooltip.Hide();
        }
    }
}
```

Prepared lines with `UITooltipLines`. Here `AddLine(string, string)` creates two columns.

```
UITooltipLines lines = new UITooltipLines();
lines.AddLine("Title", new RectOffset(), UITooltipLines.LineStyle.Title);
lines.AddLine("Strength", "+12");
lines.AddColumn("Left");
lines.AddColumn("Right");

UITooltip.InstantiateIfNecessary(this.gameObject);
UITooltip.SetLines(lines);
UITooltip.Show();
```
