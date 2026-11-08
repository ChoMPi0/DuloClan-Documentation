# Always On Top
Keeps an object above other UI when [UIUtility.BringToFront](utility.md) moves something forward, for example when a [Window](../windows/window.md) is focused. Use it for tooltips, popups and other elements that must never end up behind a window.

## Setup
1. Add **UI Always On Top** (Add Component -> UI -> Always On Top) to the object.
2. Set the **Order**. Higher orders are drawn above lower ones.

## Inspector Settings

| Property | Summary |
| --- | --- |
| Order | The order among always on top objects. Default 0. |

## Behavior
Each time `UIUtility.BringToFront` runs, it finds the active always on top objects under the same root (the canvas, or the UI Scene content). It sorts them by **Order** and moves each one to the last sibling position within its parent, lowest first. The object with the highest order ends on top.

Several built-in components add this component with fixed orders:

| Constant | Value | Used by |
| --- | --- | --- |
| ModalBoxOrder | 99996 | [Modal Box](../modal_box/modal_box.md) |
| SelectFieldBlockerOrder | 99997 | [Select Field](../controls/select_field.md) click blocker |
| SelectFieldOrder | 99998 | [Select Field](../controls/select_field.md) option list |
| TooltipOrder | 99999 | [Tooltip](../tooltips/tooltip.md) |

Use orders below 99996 to stay under these elements.

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| order | int | The order. |

## Methods

| Name | Returns | Summary |
| --- | --- | --- |
| CompareTo(object) | int | Compares by order. Used for sorting. |

## Code Usage
```
void Awake()
{
    UIAlwaysOnTop aot = this.gameObject.AddComponent<UIAlwaysOnTop>();
    aot.order = UIAlwaysOnTop.ModalBoxOrder - 1; // Below modal boxes
}
```
