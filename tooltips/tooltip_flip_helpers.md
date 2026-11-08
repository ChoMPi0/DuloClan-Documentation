# Tooltip Flip Helpers
Three small components that mirror parts of the [Tooltip](tooltip.md) when it changes the side it is shown on. They listen to the tooltip's anchor event, so ornaments, backgrounds and offset elements face the right way.

| Component | What it changes |
| --- | --- |
| UI Tooltip Flip Background | Flips a Graphic with [Flippable](../effects/flippable.md). |
| UI Tooltip Flip Effect | Flips a Graphic with Flippable and mirrors its RectTransform. |
| UI Tooltip Flip Transform | Mirrors a RectTransform only (no graphic flip). |

## How it works
Each time the tooltip updates its pivot it raises `onAnchorEvent` with the current `UITooltip.Anchor` (BottomLeft, BottomRight, TopLeft, TopRight, Left, Right, Top or Bottom). The anchor is the side or corner of the tooltip that is placed at the cursor or target.

The tooltip only raises this event when its **Anchor Graphic Properties > Graphic** is assigned. Without it the helpers do nothing.

## Setup
1. Make sure the tooltip has an **Anchor Graphic Properties > Graphic** assigned.
2. Add the helper to an object, usually inside the tooltip.
3. Assign the **Tooltip**.
4. Assign the **Graphic** (Flip Background, Flip Effect) or the **Transform** (Flip Transform).

Flip Background and Flip Effect add a **Flippable** to the graphic if it does not have one.

## Behavior

**UI Tooltip Flip Background**

| Anchor | Horizontal flip | Vertical flip |
| --- | --- | --- |
| Left | off | unchanged |
| Right | on | unchanged |
| Bottom | unchanged | off |
| Top | unchanged | on |
| BottomLeft | off | off |
| BottomRight | on | off |
| TopLeft | off | on |
| TopRight | on | on |

**UI Tooltip Flip Effect** and **UI Tooltip Flip Transform**

The original pivot, anchors and anchored position are stored on Awake.

- **Left, BottomLeft, TopLeft, Bottom**: the original values are restored. Flip Effect also turns the horizontal flip off.
- **Right, BottomRight, TopRight**: the X of the pivot and anchors is swapped (0 becomes 1, any other value becomes 0) and the X position is negated. Flip Effect also turns the horizontal flip on.
- **Top**: the same is done on the Y axis. Flip Effect also turns the vertical flip on.

Set the element up for the left / bottom case, with pivot and anchors on 0 or 1, and the helper mirrors it for the other side.
