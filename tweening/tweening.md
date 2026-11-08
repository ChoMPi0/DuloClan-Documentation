# Tweening
A small tweening system used by windows, tooltips, transitions and other components. You can use the tween structs in your own scripts.

## Tween Types

| Struct | Animates | Callback |
| --- | --- | --- |
| FloatTween | A float from **startFloat** to **targetFloat** | `UnityAction<float>` |
| ColorTween | A color from **startColor** to **targetColor**; **tweenMode** = All, RGB or Alpha | `UnityAction<Color>` |
| Vector2Tween | A Vector2 | `UnityAction<Vector2>` |
| Vector3Tween | A Vector3 | `UnityAction<Vector3>` |

Common fields: **duration** (seconds), **easing** (`TweenEasing`) and **ignoreTimeScale**.

`ignoreTimeScale` is **false** by default. Set it to true for UI that must animate while the game is paused (`Time.timeScale = 0`). All built-in components already do this.

## Easing
`TweenEasing` includes Linear, Swing and the standard In / Out / InOut variants of Quad, Cubic, Quart, Quint, Sine, Expo, Circ, Elastic, Back and Bounce.

## Running a Tween
Tweens are run by `TweenRunner<T>`, which needs a MonoBehaviour to host the coroutine.

```
using DuloGames.UI.Tweens;

public class Fader : MonoBehaviour
{
    [SerializeField] private CanvasGroup m_Group;
    private TweenRunner<FloatTween> m_Runner;

    void Awake()
    {
        this.m_Runner = new TweenRunner<FloatTween>();
        this.m_Runner.Init(this);
    }

    public void FadeTo(float alpha)
    {
        var tween = new FloatTween
        {
            startFloat = this.m_Group.alpha,
            targetFloat = alpha,
            duration = 0.3f,
            easing = TweenEasing.InOutQuint,
            ignoreTimeScale = true
        };
        tween.AddOnChangedCallback(value => this.m_Group.alpha = value);
        tween.AddOnFinishCallback(() => Debug.Log("Fade finished"));

        this.m_Runner.StartTween(tween);
    }
}
```

Starting a new tween stops the previous one on the same runner. `StopTween()` stops it without finishing.

If the host GameObject is inactive, the tween jumps to its end value and the finish callback is still called.

`TweenRunner` is `internal`; it is available to scripts compiled in the same assembly as the UI scripts (the default Assembly-CSharp).
