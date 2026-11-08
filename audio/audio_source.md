# Audio Source
A single scene-wide audio source for UI sounds with a master volume. [Play Audio](play_audio.md) and your own scripts play clips through it.

## Setup
1. Add **Audio Source** (UI/Audio/Audio Source) to one GameObject in the scene, for example the UI camera. A Unity AudioSource is added automatically.
2. Set the **Volume**.
3. Use [Play Audio](play_audio.md) or code to play clips.

Keep only one in the scene. A second one logs a warning and does not become the instance. If the instance is destroyed (for example an additive scene is unloaded), another Audio Source takes over the next time it is enabled.

The AudioSource **Play On Awake** is turned off. Clips are played with `PlayOneShot`, so several sounds can overlap.

## Inspector Settings

| Property | Summary |
| --- | --- |
| **Volume** | Master volume (0 to 1). It multiplies the volume passed to PlayAudio. |

## Methods

| Name | Returns | Summary |
| --- | --- | --- |
| PlayAudio(AudioClip) | void | Plays a clip at full volume (times the master volume). |
| PlayAudio(AudioClip, float) | void | Plays a clip at the given volume (times the master volume). |

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| Instance | UIAudioSource | Static. The active audio source, or null if there is none. |
| volume | float | The master volume. |

## Code Usage
```
[SerializeField] private AudioClip m_AudioClip;
[SerializeField][Range(0f, 1f)] private float m_Volume = 1f;

public void PlayAudio()
{
    if (this.m_AudioClip == null)
        return;

    if (UIAudioSource.Instance == null)
    {
        Debug.LogWarning("You dont have UIAudioSource in your scene. Cannot play audio clip.");
        return;
    }

    // Play the audio clip
    UIAudioSource.Instance.PlayAudio(this.m_AudioClip, this.m_Volume);
}
```

Change the UI volume from an options slider.

```
public void OnUIVolumeChanged(float value)
{
    if (UIAudioSource.Instance != null)
        UIAudioSource.Instance.volume = value;
}
```
