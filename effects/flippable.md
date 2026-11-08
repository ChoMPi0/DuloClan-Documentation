# Flippable
Mirrors a UI graphic horizontally and/or vertically without changing its scale, so layouts, anchors and child objects are not affected. Use it to reuse one sprite for left and right ornaments or arrows.

## Setup
Add **Flippable** to an object with a Graphic and tick **Horizontal** and/or **Vertical**.

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| horizontal | bool | Mirror on the X axis. |
| vertical | bool | Mirror on the Y axis. |

```
[SerializeField] private UIFlippable m_Arrow;

public void PointLeft(bool left)
{
    this.m_Arrow.horizontal = left;
}
```
