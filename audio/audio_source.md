# Audio Source
This component is used to play UI audio while allowing master volume adjustment.

## Simple usage
Attach the component to your UI camera and adjust the volume.
Use the [Play Audio component](play_audio.md) to play an AudioClip.

## Code usage
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
