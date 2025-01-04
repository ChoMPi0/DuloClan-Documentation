# Slot Cooldown
This component is used to start and keep track of slot cooldowns.

## Usage
Attach the component to any GameObject and assign the required fields.

Note that the **Target Slot** must implement the **IUISlotHasCooldown** interface.

The cooldown must be started manually from code, it's preferable to have a dedicated cooldown manager for that.

You can access the cooldown component from the slot's **cooldownComponent** property.

```
public void StartSlotCooldown(UISpellSlot slot)
{
    // Make sure we have the cooldown component
    if (slot.cooldownComponent == null)
        return;

    // Make sure the slot is assigned
    if (!slot.IsAssigned())
        return;

    // Get the spell info from the slot
    UISpellInfo spellInfo = slot.GetSpellInfo();

    // Handle cooldown just for the demonstration
    if (spellInfo.Cooldown > 0f)
    {
        // Start the cooldown
        slot.cooldownComponent.StartCooldown(spellInfo.ID, spellInfo.Cooldown);
    }
}
```

Here is an example of the demo cast manager we have in our assets.

```
using UnityEngine;

namespace DuloGames.UI
{
    public class Demo_CastManager : MonoBehaviour
    {
        [SerializeField] private UICastBar m_CastBar;
        [SerializeField] private Transform[] m_SlotContainers;

        protected void Start()
        {
            if (this.m_SlotContainers.Length > 0)
            {
                foreach (Transform t in this.m_SlotContainers)
                {
                    UISpellSlot[] slots = t.GetComponentsInChildren();

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
    }
}
```
