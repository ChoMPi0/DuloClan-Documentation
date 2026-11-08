# Highlight Transition
This component adds an extra transition target to common controls like **Button**, **Toggle**, **Slider** and such. Unity's Selectable drives one target graphic; add Highlight Transitions to animate more (a glow, a label color, an icon).

## Setup
1. Add **Highlight Transition** to the control (the object that receives the pointer events).
2. Choose the **Transition** and assign its target.
3. Set the values for each state.

## Transition Types

| Type | Target | Values per state |
| --- | --- | --- |
| Color Tint | Graphic | Colors, color multiplier and duration |
| Sprite Swap | Image | Sprites |
| Animation | GameObject with an Animator | Trigger names |
| Text Color | TextMeshPro text | Colors and duration |
| Canvas Group | CanvasGroup | Alpha values and duration |

## States
**Normal**, **Highlighted** (pointer over), **Selected** (keyboard/gamepad focus), **Pressed** and **Active**.

The **Active** state is used with **Use Toggle**: assign a **Target Toggle** and the transition shows Active while the toggle is on, which is useful for tabs and toggle buttons.

All transitions run on unscaled time, so they keep working while the game is paused (`Time.timeScale = 0`).

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| transition | Transition | The transition type. |
| targetGraphic | Graphic | Target for Color Tint, Sprite Swap and Text Color. |
| targetGameObject | GameObject | Target for Animation. |
| targetCanvasGroup | CanvasGroup | Target for Canvas Group. |
| animator | Animator | The Animator on the target GameObject. |

See also [Press Transition](press_transition.md), [Effect Transition](effect_transition.md) and [Toggle Active Transition](toggle_active_transition.md).
