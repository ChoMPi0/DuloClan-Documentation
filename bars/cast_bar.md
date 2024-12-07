# Cast Bar
Simple cast bar used to start or interrupt a spell cast.

## Properties
| Name | Type | Summary |
| --- | --- | --- |
| canvasGroup |	CanvasGroup	| Gets the canvas group. |
| IsCasting	| bool | Gets a value indicating whether this cast bar is casting. |

## Object Oriented Methods
| Name | Returns | Summary |
| --- | --- | --- |
| ApplyColorStage(UICastBar.ColorStage) | void | Applies the color stages. |
| Show() | void |	Show this cast bar. |
| Show(bool) | void	| Show this cast bar. |
| Hide() | void	| Hide this cast bar. |
| Hide(bool) | void	| Hide this cast bar. |
| Interrupt() | void	| Interrupts the current cast if any. |
| SetCanvasAlpha(float) | void	| Sets the canvas alpha. |
| SetFillAmount(float) | void	| Sets the fill amount. |
| StartAlphaTween(float, float, bool) | void	| Starts alpha tween. |
| StartCasting(UISpellInfo, float, float) | void	| Starts the casting of the specified spell. |

## Code Usage
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

    // Make sure we have spell info
    if (spellInfo == null)
        return;

    // Check if we are on cooldown
    if (slot.cooldownComponent != null && slot.cooldownComponent.IsOnCooldown)
        return;

    // Check if the spell is not insta cast
    if (!spellInfo.Flags.Has(UISpellInfo_Flags.InstantCast))
    {
        // Start casting
        this.m_CastBar.StartCasting(spellInfo, spellInfo.CastTime, Time.time + spellInfo.CastTime);
    }

    // Handle cooldown just for the demonstration
    if (slot.cooldownComponent != null && spellInfo.Cooldown > 0f)
    {
        // Start the cooldown on all the slots with the specified spell id
        foreach (UISpellSlot s in UISpellSlot.GetSlots())
        {
            if (s.IsAssigned() && s.GetSpellInfo() != null && s.cooldownComponent != null)
            {
                // If the slot IDs match
                if (s.GetSpellInfo().ID == spellInfo.ID)
                {
                    // Start the cooldown
                    s.cooldownComponent.StartCooldown(spellInfo.ID, spellInfo.Cooldown);
                }
            }
        }
    }
}
```
