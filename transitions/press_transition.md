# Press Transition
This component adds an extra transition target that reacts only to the pressed state of common controls like **Button**, **Toggle**, **Slider** and such. Use it for a press effect (an icon that darkens, a label that shifts color) without changing the hover look.

## Setup
1. Add **Press Transition** to the control.
2. Choose the **Transition** and assign its target.
3. Set the **Normal** and **Pressed** values.

## Transition Types

| Type | Target | Values |
| --- | --- | --- |
| Color Tint | Graphic | Normal / pressed colors, color multiplier, duration |
| Sprite Swap | Image | Pressed sprite |
| Animation | GameObject with an Animator | Normal / pressed trigger names |
| Text Color | TextMeshPro text | Normal / pressed colors, duration |

Transitions run on unscaled time and work while the game is paused.

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| transition | Transition | The transition type. |
| targetGraphic | Graphic | Target for Color Tint, Sprite Swap and Text Color. |
| targetGameObject | GameObject | Target for Animation. |
| animator | Animator | The Animator on the target GameObject. |

See also [Highlight Transition](highlight_transition.md).
