# Toggle Active Transition
This component transitions a target between **Normal** and **Active** following a **Toggle**'s value. Use it for checkmarks, selected tab highlights or any visual that should only reflect on/off.

## Setup
1. Add **Toggle Active Transition** next to a **Toggle** (or assign a **Target Toggle**).
2. Choose the **Transition** and assign its target.
3. Set the Normal and Active values and the **Duration**.

## Transition Types

| Type | Target | Values |
| --- | --- | --- |
| Color Tint | Graphic | Normal / active colors (normal is transparent by default) |
| Sprite Swap | Image | Active sprite |
| Animation | GameObject with an Animator | Normal trigger and the **Active** bool parameter |
| Text Color | Text | Normal / active colors |
| Canvas Group | CanvasGroup | Normal / active alpha (0 and 1 by default) |

The visual follows the toggle whether it is changed by the user or from code.

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| transition | Transition | The transition type. |
| targetGraphic | Graphic | Target for Color Tint, Sprite Swap and Text Color. |
| targetGameObject | GameObject | Target for Animation. |
| targetCanvasGroup | CanvasGroup | Target for Canvas Group. |

See also [Highlight Transition](highlight_transition.md).
