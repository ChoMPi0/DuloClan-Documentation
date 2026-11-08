# Play Audio
Plays an AudioClip when a pointer event happens on a UI element, such as a click or hover sound on a button.

It plays through the [Audio Source](audio_source.md). Make sure one is in the scene, preferably on your UI camera. Without it a warning is logged and nothing plays.

## Setup
1. Add **Play Audio** (UI/Audio/Play Audio) to a UI element that receives pointer events (a Graphic with Raycast Target on, such as an Image or Text).
2. Assign the **Audio Clip**.
3. Set the **Volume**.
4. Choose the **Play On Event**.

Use one Play Audio per sound. For a hover and a click sound, add two components.

## Inspector Settings

| Property | Summary |
| --- | --- |
| **Audio Clip** | The clip to play. |
| **Volume** | Clip volume (0 to 1). It is multiplied by the Audio Source volume. |
| **Play On Event** | The event that plays the clip. |

## Play On Event

| Event | Plays when |
| --- | --- |
| None | Never (call PlayAudio from code or a UnityEvent). |
| PointerEnter | The pointer enters, while no button is held. |
| PointerExit | The pointer exits, while no button is held. |
| PointerDown | The left button is pressed. |
| PointerUp | The left button is released. |
| Click | The left button is pressed and released (single click). |
| DoubleClick | The left button is released on a double click. |

Only the left mouse button triggers the down, up, click and double click events.

## Methods

| Name | Returns | Summary |
| --- | --- | --- |
| PlayAudio() | void | Plays the clip now (if the component is enabled and active). |

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| audioClip | AudioClip | The clip to play. |
| volume | float | The clip volume. |
| playOnEvent | UIPlayAudio.Event | The event that plays the clip. |

## Code Usage
```
[SerializeField] private UIPlayAudio m_ClickSound;
[SerializeField] private AudioClip m_ErrorClip;

public void OnInvalidAction()
{
    // Swap the clip and play it now
    this.m_ClickSound.audioClip = this.m_ErrorClip;
    this.m_ClickSound.PlayAudio();
}
```
