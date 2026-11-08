# Tab
A Toggle that shows its **Target Content** while it is on and hides it while it is off. Use a set of tabs to switch between pages of a window.

## Setup
1. Place the tabs under one parent.
2. Add **Tab** (UI/Tab) to each tab button.
3. Assign each tab's page to **Target Content**.
4. Set **Is On** on the tab that should be open by default.
5. Optionally set up the **Image Target** and **Text Target** transitions.

If **Group** is empty, the tab looks for a ToggleGroup in its parents. If none is found, it adds a ToggleGroup to its parent. Tabs under the same parent share that group, so only one is on at a time.

## Inspector Settings

| Property | Summary |
| --- | --- |
| **Target Content** | GameObject activated while the tab is on and deactivated while it is off. |
| **Image Target** | Image that transitions with the tab state. |
| **Image Transition** | None, Color Tint, Sprite Swap or Animation. |
| **Image Colors**, **Image Sprite State**, **Image Animation Triggers** | Values per state. They include Active states used while the tab is on. |
| **Text Target** | Text that transitions with the tab state. |
| **Text Transition** | None or ColorTint. |
| **Text Colors** | Text colors per state, including Active states. |
| **Is On**, **Transition**, **Graphic**, **Group** | Standard Toggle properties. Child graphics of **Graphic** also fade in and out with the tab state. |

## Events
`onValueChanged` is the standard Toggle `UnityEvent<bool>`.

```
public void OnTabStateChanged(bool state)
{
    Debug.Log("Tab state: " + state);
}
```

## Methods

| Name | Returns | Summary |
| --- | --- | --- |
| Activate() | void | Turns the tab on (if it is off). |
| EvaluateAndToggleContent() | void | Shows or hides the **Target Content** to match the tab state. |

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| targetContent | GameObject | The content shown while the tab is on. |
| imageTarget | Image | The image transition target. |
| imageTransition | Selectable.Transition | The image transition type. |
| imageColors | ColorBlockExtended | The image colors. |
| imageSpriteState | SpriteStateExtended | The image sprites. |
| imageAnimationTriggers | AnimationTriggersExtended | The image animation triggers. |
| textTarget | Text | The text transition target. |
| textTransition | UITab.TextTransition | The text transition type. |
| textColors | ColorBlockExtended | The text colors. |

## Code Usage
```
[SerializeField] private UITab m_InventoryTab;

public void OpenInventoryTab()
{
    if (this.m_InventoryTab != null)
        this.m_InventoryTab.Activate();
}
```

To open a window and a tab together, see [Toggle Window and Tab](../miscellaneous/toggle_window_and_tab.md).
