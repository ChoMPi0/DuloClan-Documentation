# Tooltip Manager
This is a scriptable object which handles the instantiation of [Tooltip](tooltip.md). The manager also contains the line styles and few other settings.

## Simple Usage
The manager must be placed in a Resources directory in your project.

To create the manager right click in your Project view then select Create -> UI Managers -> Tooltip Manager.

Configure the text line styles that are going to be used by the tooltip.

Assign the [Tooltip](tooltip.md) prefab.

## Code Usage
You can access the tooltip manager at any time by the Instance static property.

```
public void InstantiateIfNecessary(GameObject rel)
{
    if (UITooltipManager.Instance == null || UITooltipManager.Instance.prefab == null)
        return;

    // Get the canvas
    Canvas canvas = UIUtility.FindInParents(rel);

    if (canvas == null)
        return;

    // Instantiate a tooltip
    Instantiate(UITooltipManager.Instance.prefab, canvas.transform, false);
}
```
