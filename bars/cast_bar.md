# Cast Bar
A cast bar that shows a spell being cast: its name, icon, time and a fill. Start it with a spell and a duration; it fills, then hides. It can be interrupted.

## Setup
1. Add **UI Cast Bar** to the cast bar object. A CanvasGroup is added automatically.
2. Assign a [Progress Bar](progress_bar.md) to **Progress Bar**. It drives the fill.
3. Assign the **Fill Image**. It is tinted by the color stages.
4. Assign the **Spell Title** and **Time Text** labels. Optionally assign a **Full Time Text** label.
5. Optionally turn on **Use Icon** and assign the icon **Frame** and **Image**.
6. Set the **Colors**, **Show Transition** and **Hide Transition**.

In play mode the bar starts hidden (canvas alpha 0).

## Inspector Settings

| Section | Property | Summary |
| --- | --- | --- |
| | **Progress Bar** | The progress bar used for the fill. |
| | **Fill Image** | The image tinted by the color stages. |
| Spell Title | **Text** | Shows the spell name. |
| Time Text | **Text** | Shows the cast time. |
| Time Text | **Display** | Elapsed or Remaining time. |
| Time Text | **Format** | Number format, default `0.0 sec`. |
| Full Time Text | **Text**, **Format** | Shows the spell's full cast time (`UISpellInfo.CastTime`). |
| Spell Icon | **Use Icon** | Show the spell icon. |
| Spell Icon | **Frame**, **Image** | The icon frame (activated while showing) and image. |
| Colors | **Normal**, **On Interrupt**, **On Finish** | Color stages: fill, title and time colors. |
| Show Transition | **Transition**, **Duration** | Instant or Fade when shown. |
| Show Transition | **Bring to Front** | Moves the bar to the front when shown. |
| Hide Transition | **Transition**, **Duration** | Instant or Fade when hidden. |
| Hide Transition | **Hide Delay** | Seconds to wait after finish or interrupt before hiding. |

## Casting
`StartCasting(spellInfo, duration, endTime)` starts a cast:
- **duration** is the cast length in seconds.
- **endTime** is when the cast ends, in `Time.time`. The start is `endTime - duration`, so a cast already in progress (for example from the server) starts partly filled. Pass 0 to start now.

It does nothing while a cast is running. Check `IsCasting` first or call `Interrupt()`.

Color stages:
- **Normal** is applied when a cast starts.
- **On Finish** is applied when the cast completes.
- **On Interrupt** is applied when `Interrupt()` is called during a cast.

After finish or interrupt the bar waits **Hide Delay** seconds, then hides.

## Methods

| Name | Returns | Summary |
| --- | --- | --- |
| StartCasting(UISpellInfo, float, float) | void | Starts casting a spell with a duration and end time. |
| Interrupt() | void | Interrupts the current cast, if any. |
| Show() | void | Shows the bar with the show transition. |
| Show(bool) | void | Shows the bar; true shows it instantly. |
| Hide() | void | Hides the bar with the hide transition. |
| Hide(bool) | void | Hides the bar; true hides it instantly. |
| ApplyColorStage(UICastBar.ColorStage) | void | Applies a color stage to the fill, title and time. |
| SetFillAmount(float) | void | Sets the progress bar fill amount. |
| StartAlphaTween(float, float, bool) | void | Tweens the canvas alpha (target, duration, ignore time scale). |

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| IsCasting | bool | Is a cast running. |
| canvasGroup | CanvasGroup | The canvas group. |

## Code Usage
Start a cast from a spell slot click. Instant cast spells skip the bar.

```
[SerializeField] private UICastBar m_CastBar;

public void OnSpellClick(UISpellSlot slot)
{
    // Make sure we have the cast bar component and the slot is assigned
    if (this.m_CastBar == null || !slot.IsAssigned())
        return;

    // Check if we are already casting
    if (this.m_CastBar.IsCasting)
        return;

    // Get the spell info from the slot
    UISpellInfo spellInfo = slot.GetSpellInfo();

    if (spellInfo == null)
        return;

    // Check if we are on cooldown
    if (slot.cooldownComponent != null && slot.cooldownComponent.IsOnCooldown)
        return;

    // Start casting if the spell is not instant cast
    if (!spellInfo.Flags.Has(UISpellInfo_Flags.InstantCast))
        this.m_CastBar.StartCasting(spellInfo, spellInfo.CastTime, Time.time + spellInfo.CastTime);
}
```

Interrupt the cast, for example when the player moves.

```
public void OnPlayerMoved()
{
    if (this.m_CastBar != null && this.m_CastBar.IsCasting)
        this.m_CastBar.Interrupt();
}
```

See the [Spell Slot](../icon_slots/spell_slot.md) and [Databases](../icon_slots/databases.md) pages for `UISpellSlot` and `UISpellInfo`.
