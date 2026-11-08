# Slot Cooldown
Shows a cooldown on a [Spell Slot](spell_slot.md): a filled image, a countdown text and an optional moving edge graphic. Cooldowns are stored per spell ID, so moving a spell to another slot keeps its remaining time.

## Setup
1. Add **Slot Cooldown** (Add Component -> UI -> Icon Slots -> Slot Cooldown) to the slot or a child.
2. Assign the slot to **Target Slot**. It must implement `IUISlotHasCooldown` ([Spell Slot](spell_slot.md) does). Otherwise a warning is logged and the component disables itself.
3. Assign **Target Graphic**, an **Image** with **Image Type** set to **Filled**. Its fill amount runs from 1 to 0.
4. Optionally assign **Target Text** for the remaining seconds.
5. Optionally assign **Finish Graphic**, an edge that moves down with the fill.
6. Start cooldowns from code with `StartCooldown`.

The graphics and text are hidden when no cooldown is running.

## Properties

| Property | Summary |
| --- | --- |
| Target Slot | The slot this cooldown belongs to. |
| Target Graphic | Filled image that shows the remaining time. |
| Target Text | Text with the remaining seconds. |
| Finish Graphic | Optional edge graphic that follows the fill. |
| Finish Offset Y | Vertical offset of the finish graphic. |
| Finish Fading Pct | Percent of the duration at the start and end where the finish graphic fades. |

## Behavior
- `StartCooldown(spellId, duration)` starts the display on this slot and records the end time for that spell ID. The record is shared by all slots.
- A slot resumes the remaining time when a spell with a running cooldown is assigned to it, or when the cooldown object is enabled again.
- Other slots that already show the same spell are not updated automatically. Call `StartCooldown` on each of them (see the demo below).
- Unassigning the spell or disabling the object stops the display. The shared record is kept until it expires.

## Methods

| Name | Returns | Summary |
| --- | --- | --- |
| StartCooldown(int, float) | void | Starts a cooldown for the spell ID with a duration in seconds. Needs a **Target Graphic**. |
| ResumeCooldown(int) | void | Resumes the display of a running cooldown for the spell ID. |
| CheckForActiveCooldown() | void | Resumes the cooldown of the assigned spell, if one is running. |
| InterruptCooldown() | void | Stops the display on this slot. |
| OnAssignSpell() | void | Called by the slot when a spell is assigned. |
| OnUnassignSpell() | void | Called by the slot when the spell is unassigned. |

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| IsOnCooldown | bool | Is a cooldown displayed on this slot (read only). |

## Code Usage
The slot exposes its cooldown through `cooldownComponent`.

```
public void StartSlotCooldown(UISpellSlot slot)
{
    // Make sure we have the cooldown component and a spell
    if (slot.cooldownComponent == null || !slot.IsAssigned())
        return;

    UISpellInfo spellInfo = slot.GetSpellInfo();

    if (spellInfo.Cooldown > 0f)
        slot.cooldownComponent.StartCooldown(spellInfo.ID, spellInfo.Cooldown);
}
```

The demo cast manager included with the asset. It starts the cooldown on every slot that holds the clicked spell.

```
using UnityEngine;

namespace DuloGames.UI
{
    public class Demo_CastManager : MonoBehaviour
    {
        #pragma warning disable 0649
        [SerializeField] private UICastBar m_CastBar;
        [SerializeField] private Transform[] m_SlotContainers;
        #pragma warning restore 0649

        protected void Start()
        {
            if (this.m_SlotContainers != null && this.m_SlotContainers.Length > 0)
            {
                foreach (Transform t in this.m_SlotContainers)
                {
                    UISpellSlot[] slots = t.GetComponentsInChildren<UISpellSlot>();

                    foreach (UISpellSlot slot in slots)
                    {
                        slot.onClick.AddListener(OnSpellClick);
                    }
                }
            }
        }

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
            if (spellInfo.Cooldown > 0f && slot.cooldownComponent != null && slot.cooldownComponent.IsOnCooldown)
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
                        // If the spell IDs match
                        if (s.GetSpellInfo().ID == spellInfo.ID)
                        {
                            // Start the cooldown
                            s.cooldownComponent.StartCooldown(spellInfo.ID, spellInfo.Cooldown);
                        }
                    }
                }
            }
        }
    }
}
```

See also [Cast Bar](../bars/cast_bar.md).
