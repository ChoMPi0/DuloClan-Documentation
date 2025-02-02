# Tooltip
Easily create a nicely looking tooltip.

## Simple Usage
Attach the component to any GameObject and assign the required fields.

After configuring your tooltip, create a prefab of it and assign it to the [Tooltip Manager](tooltip_manager.md).

Make sure the line styles are configured in the [Tooltip Manager](tooltip_manager.md).

The visibility of the tooltip is controlled by a CanvasGroup alpha value.

To display a tooltip you could use the [Tooltip Show](tooltip_show.md) component or via code.

## Code Usage
Always call the **InstantiateIfNecessary(GameObject go)** method before using the static methods. This will make sure you have the tooltip instantiated in the current canvas.

Use the **Show()** and **Hide()** methods to toggle the visibility of the tooltip.

Every time the tooltip hides the text lines are cleared.

If you instantiate a tooltip into the scene manually that instance is going to be used for that scene.

The tooltip is designed to work with a single instance and most methods are static.

```
public void OnTooltip(bool show)
{
    // Check if we have the instance of the manager
    if (UITooltipManager.Instance == null)
        return;
        
    // If we are showing the tooltip
    if (show)
    {
        UITooltip.InstantiateIfNecessary(this.gameObject);
        
        // Add a simple title line
        UITooltip.AddTitle("Tooltip Title");
        
        // Add a spacer
        UITooltip.AddSpacer();
        
        // Add a simple line of text
        UITooltip.AddLine("A line of text");
        
        // Add columns of text
        UITooltip.AddLineColumn("Left column text");
        UITooltip.AddLineColumn("Right column text");
        
        // Add description line
        UITooltip.AddDescription("Some very simple description");
        
        // You can set the tooltip width
        UITooltip.SetWidth(512f);

        // You can set the tooltip horizontal fit mode
        UITooltip.SetHorizontalFitMode(ContentSizeFitter.FitMode.PreferredSize);

        // You can anchor the tooltip to a transform
        UITooltip.AnchorToRect(this.transform as RectTransform);

        // You can override the tooltip offset
        UITooltip.OverrideOffset(Vector2.zero);
        UITooltip.OverrideAnchoredOffset(Vector2.zero);

        // Show the tooltip
        UITooltip.Show();
    }
    else
    {
        // Hide the tooltip
        UITooltip.Hide();
    }
}
```

## Static Methods


| Name  | Returns | Summary |
| ------------- | ------------- | ------------- |
| AddTitle(string) | void | Adds title line. |
| AddDescription(string) | void | Adds description line. |
| AddLine(string) | void | Adds a new single column line. |
| AddLine(string, RectOffset) | void | Adds a new single column line. |
| AddLine(string, RectOffset, string) | void | Adds a new single column line. |
| AddLine(string, RectOffset, UITooltipLines.LineStyle) | void | Adds a new single column line. |
| AddLine(string, string) | void | Adds a new single column line. |
| AddLine(string, UITooltipLines.LineStyle) | void | Adds a new single column line. |
| AddLineColumn(string) | void | Adds a column (Either to the last line if it's not complete or to a new one). |
| AddLineColumn(string, string) | void | Adds a column (Either to the last line if it's not complete or to a new one). |
| AddLineColumn(string, UITooltipLines.LineStyle) | void | Adds a column (Either to the last line if it's not complete or to a new one). |
| AddSpacer() | void | Adds a spacer line. |
| AnchorToRect(RectTransform) | void | Anchors the tooltip to a rect. |
| Show() | void | Show the tooltip. |
| Hide() | void | Hide the tooltip. |
| InstantiateIfNecessary(GameObject) | void | Instantiate the tooltip game object if necessary. |
| OverrideAnchoredOffset(Vector2) | void | Overrides the anchored offset for single display of the tooltip. |
| OverrideOffset(Vector2) | void | Overrides the offset for single display of the tooltip. |
| SetHorizontalFitMode(ContentSizeFitter.FitMode) | void | Sets the horizontal fit mode of the tooltip. |
| SetLines(UITooltipLines) | void | Sets the lines template. |
| SetWidth(float) | void | Sets the width of the toolip. |
