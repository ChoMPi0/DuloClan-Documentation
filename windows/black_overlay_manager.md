# Black Overlay Manager
A ScriptableObject that holds the [Black Overlay](black_overlay.md) prefab. [Windows](window.md) use it to create an overlay in canvases that do not have one.

## Setup
1. Right click in the Project view and select Create -> UI Managers -> Black Overlay Manager.
2. Save the asset in a **Resources** folder and keep the default name **BlackOverlayManager**. It is loaded with `Resources.Load("BlackOverlayManager")`, so a renamed asset or one outside a Resources folder is not found.
3. Assign the [Black Overlay](black_overlay.md) prefab to **Black Overlay Prefab**.

## Inspector Settings

| Property | Summary |
| --- | --- |
| Black Overlay Prefab | The prefab instantiated as the overlay. It must have a UIBlackOverlay component. |

## Methods

| Name | Returns | Summary |
| --- | --- | --- |
| Create(Transform) | UIBlackOverlay | Instantiates the prefab under the given parent. Returns null when no prefab is assigned. |

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| prefab | GameObject | The black overlay prefab. |

## Static Properties

| Name | Type | Summary |
| --- | --- | --- |
| Instance | UIBlackOverlayManager | The manager asset loaded from Resources, or null. |

## Code Usage
In most cases use `UIBlackOverlay.GetOverlay`, which reuses an existing overlay. This example creates one directly.

```
public UIBlackOverlay CreateOverlay()
{
    Canvas canvas = UIUtility.FindInParents<Canvas>(this.gameObject);

    if (canvas == null || UIBlackOverlayManager.Instance == null)
        return null;

    return UIBlackOverlayManager.Instance.Create(canvas.transform);
}
```
