# Slot Base
The base class of all icon slots: [Item Slot](item_slot.md), [Spell Slot](spell_slot.md), [Equip Slot](equip_slot.md) and [Talent Slot](talent_slot.md). It handles the icon, drag and drop, the tooltip delay and the hover and press transitions. Use it directly for icon-only slots, or extend it for your own data (see [How To Extend](#how-to-extend)).

## Setup
1. Create a slot object under your canvas with a background graphic that can receive raycasts.
2. Add a child **Image** (or **Raw Image**) for the icon.
3. Add **Base Slot** (Add Component -> UI -> Icon Slots -> Base Slot), or one of the derived slots.
4. Assign the icon to **Icon Graphic**.
5. Optionally assign a **Clone Target**. It is cloned and follows the pointer while dragging. When empty, the icon graphic is cloned.
6. Set the drag and drop, tooltip and transition properties below.

An unassigned slot hides the icon object on start.

## Properties

| Property | Summary |
| --- | --- |
| Icon Graphic | The **Image** or **Raw Image** that shows the icon. |
| Clone Target | Object cloned as the drag icon. Defaults to the icon graphic. |
| Enabled (Drag & Drop) | Allows dragging from and dropping on this slot. |
| Drag Key Modifier | **None**, **Control**, **Alt** or **Shift**. When set, the key must be held to start a drag. |
| Is Static | A static slot keeps its icon when dragged from, and does not accept drops when assigned. Use it for spell books and vendors. |
| Allow Throw Away | When on, dropping the icon outside any slot unassigns the slot. When off, the slot's throw away denied handler runs instead. |
| Enabled (Tooltip) | Shows the tooltip on hover. |
| Display Delay | Seconds the pointer must stay over the slot before the tooltip shows. 0 shows instantly. |
| Hovered State: Target Graphic / Transition | Graphic and transition (**None**, **Color Tint**, **Sprite Swap**, **Animation**) for the hover state. |
| Pressed State: Target Graphic / Transition | Graphic and transition for the pressed state. |
| Instant Out | Return from the pressed color instantly on release (Color Tint). |
| Force Hover Normal | Show the hover target in its normal state while pressed. |

## Drag and Drop
- A drag starts only when the slot is assigned, drag and drop is enabled and the **Drag Key Modifier** key is held.
- Dropping on an empty slot assigns it. The source is unassigned unless it is static.
- Dropping on an assigned slot swaps them when neither is static and both accept the swap (`CanSwapWith`). If only the source is static, the target is replaced.
- Releasing over the same slot does nothing.
- Releasing anywhere else, where no slot or [Equip Receiver](equip_receiver.md) takes the drop, throws the icon away (see **Allow Throw Away**).

## Tooltip
The tooltip opens after **Display Delay** and closes when the pointer exits or presses the slot. Slot Base calls `OnTooltip(bool)`, which is empty here. Derived slots override it to fill the [Tooltip](../tooltips/tooltip.md).

## Methods

| Name | Returns | Summary |
| --- | --- | --- |
| IsAssigned() | bool | Does the slot have an icon. Derived slots check their info instead. |
| Assign(Sprite) | bool | Sets a sprite icon. Needs an **Image** icon graphic. |
| Assign(Texture) | bool | Sets a texture icon. Needs a **Raw Image** icon graphic. |
| Assign(Object) | bool | Assigns from another slot. Virtual. |
| Unassign() | void | Clears the slot. Virtual. |
| GetIconSprite() | Sprite | The icon sprite, or null. |
| GetIconTexture() | Texture | The icon texture, or null. |
| GetIconAsObject() | Object | The icon sprite or texture. |
| SetIcon(Sprite) | void | Sets the sprite and shows or hides the icon object. |
| SetIcon(Texture) | void | Sets the texture and shows or hides the icon object. |
| ClearIcon() | void | Removes the icon and hides the icon object. |
| CanSwapWith(Object) | bool | Can this slot swap with the target. Virtual. |
| PerformSlotSwap(Object) | bool | Swaps the content with another slot. Virtual. |
| DragKeyModifierIsDown() | bool | Is the drag key modifier held (true for **None**). |
| OnTooltip(bool) | void | Called to show or hide the tooltip. Virtual, empty in the base. |

Protected virtual methods for derived slots: `OnThrowAway()`, `OnThrowAwayDenied()`, `OnAssignBySlotFailed(Object)` and `CreateTemporaryIcon(PointerEventData)`. The pointer and drag handlers (`OnPointerClick`, `OnBeginDrag`, `OnDrop`, ...) are public and virtual.

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| iconGraphic | Graphic | The icon graphic (public field). |
| dragAndDropEnabled | bool | Drag and drop enabled. |
| dragKeyModifier | UISlotBase.DragKeyModifier | The key needed to start a drag. |
| isStatic | bool | Is the slot static. |
| allowThrowAway | bool | Allow throw away. |
| tooltipEnabled | bool | Tooltip enabled. |
| tooltipDelay | float | The tooltip delay in seconds. |
| pressTransitionInstaOut | bool | Instant out for the pressed state. |
| pressForceHoverNormal | bool | Force hover normal while pressed. |
| dragHasBegan | bool | Is a drag in progress (read only). |
| dropPreformed | bool | Was the drag dropped on a receiver. |

## Code Usage
```
using UnityEngine;
using DuloGames.UI;

public class AssignIcon : MonoBehaviour
{
    [SerializeField] private UISlotBase m_Slot;
    [SerializeField] private Sprite m_Icon;

    void Start()
    {
        this.m_Slot.dragKeyModifier = UISlotBase.DragKeyModifier.Shift;
        this.m_Slot.tooltipDelay = 0.5f;
        this.m_Slot.Assign(this.m_Icon);
    }
}
```

## How To Extend
Let's build a new slot by extending the **Slot Base** class.

```
using UnityEngine;
using UnityEngine.Events;
using UnityEngine.EventSystems;
using System;
using DuloGames.UI;
using Object = UnityEngine.Object;

public class MyCustomSlot : UISlotBase { }
```

Now define a class to hold our custom slot info, inside `MyCustomSlot`.

```
public class MyCustomInfo
{
    public int id;
    public Sprite icon;
    public string name;
}

/// <summary>
/// The assigned custom info.
/// </summary>
private MyCustomInfo m_MyCustomInfo;

/// <summary>
/// Gets the info assigned to this slot.
/// </summary>
/// <returns>The info.</returns>
public MyCustomInfo GetInfo()
{
    return this.m_MyCustomInfo;
}
```

To keep track of what the slots contain we add assign and unassign events, so we can save that info. To use the slot we also need a click event.

Here is how to define the events.

```
[Serializable] public class OnClickEvent : UnityEvent<MyCustomSlot> { }
[Serializable] public class OnAssignEvent : UnityEvent<MyCustomSlot> { }
[Serializable] public class OnUnassignEvent : UnityEvent<MyCustomSlot> { }

/// <summary>
/// The click event delegate.
/// </summary>
public OnClickEvent onClick = new OnClickEvent();

/// <summary>
/// The assign event delegate.
/// </summary>
public OnAssignEvent onAssign = new OnAssignEvent();

/// <summary>
/// The unassign event delegate.
/// </summary>
public OnUnassignEvent onUnassign = new OnUnassignEvent();
```

To check if the slot is assigned, override the **IsAssigned()** method.

```
/// <summary>
/// Determines whether this slot is assigned.
/// </summary>
/// <returns><c>true</c> if this instance is assigned; otherwise, <c>false</c>.</returns>
public override bool IsAssigned()
{
    return (this.m_MyCustomInfo != null);
}
```

To handle assigning from another slot (drag and drop), override the **Assign(Object source)** method. We also add a method to assign the slot from other scripts.

```
/// <summary>
/// Assign the slot by the passed source slot.
/// </summary>
/// <param name="source">Source.</param>
public override bool Assign(Object source)
{
    if (source is MyCustomSlot)
    {
        MyCustomSlot sourceSlot = source as MyCustomSlot;

        if (sourceSlot != null)
            return this.Assign(sourceSlot.GetInfo());
    }

    // Default
    return false;
}

/// <summary>
/// Assign the slot by info.
/// </summary>
/// <param name="info">The info.</param>
public bool Assign(MyCustomInfo info)
{
    if (info == null)
        return false;

    // Make sure we unassign first, so the event is called before new assignment
    this.Unassign();

    // Use the base class assign to set the icon
    this.Assign(info.icon);

    // Set the info
    this.m_MyCustomInfo = info;

    // Invoke the on assign event
    if (this.onAssign != null)
        this.onAssign.Invoke(this);

    // Success
    return true;
}
```

Now the unassign.

```
/// <summary>
/// Unassign this slot.
/// </summary>
public override void Unassign()
{
    // Remove the icon
    base.Unassign();

    // Clear the info
    this.m_MyCustomInfo = null;

    // Invoke the on unassign event
    if (this.onUnassign != null)
        this.onUnassign.Invoke(this);
}
```

Next, override the swapping methods to handle swapping between our custom slots. When a slot is dropped on an assigned slot, `PerformSlotSwap` is called on the dragged slot with the slot it was dropped on.

```
/// <summary>
/// Determines whether this slot can swap with the specified target slot.
/// </summary>
/// <returns><c>true</c> if this instance can swap with the specified target; otherwise, <c>false</c>.</returns>
/// <param name="target">Target.</param>
public override bool CanSwapWith(Object target)
{
    // We can only swap with another MyCustomSlot
    return (target is MyCustomSlot);
}

/// <summary>
/// Performs a slot swap.
/// </summary>
/// <returns><c>true</c>, if slot swap was performed, <c>false</c> otherwise.</returns>
/// <param name="targetObject">The other slot.</param>
public override bool PerformSlotSwap(Object targetObject)
{
    // Get the other slot
    MyCustomSlot targetSlot = (targetObject as MyCustomSlot);

    // Get the other slot info
    MyCustomInfo targetInfo = targetSlot.GetInfo();

    // Assign the other slot by this slot
    bool assign1 = targetSlot.Assign(this.GetInfo());

    // Assign this slot by the other slot info
    bool assign2 = this.Assign(targetInfo);

    // Return the status
    return (assign1 && assign2);
}
```

To handle clicks, override the **OnPointerClick(PointerEventData eventData)** method.

```
/// <summary>
/// Raises the pointer click event.
/// </summary>
/// <param name="eventData">Event data.</param>
public override void OnPointerClick(PointerEventData eventData)
{
    base.OnPointerClick(eventData);

    // Make sure the slot is assigned
    if (!this.IsAssigned())
        return;

    // Check for left click
    if (eventData.button == PointerEventData.InputButton.Left)
    {
        // Invoke the click event
        if (this.onClick != null)
            this.onClick.Invoke(this);
    }
}
```

Finally, the tooltip. `OnTooltip` is called after the **Display Delay**.

```
/// <summary>
/// Raises the tooltip event.
/// </summary>
/// <param name="show">If set to <c>true</c> show.</param>
public override void OnTooltip(bool show)
{
    // Make sure we have info
    if (this.m_MyCustomInfo == null)
        return;

    // If we are showing the tooltip
    if (show)
    {
        UITooltip.InstantiateIfNecessary(this.gameObject);

        // Set the title
        UITooltip.AddTitle(this.m_MyCustomInfo.name);

        // Anchor to this slot
        UITooltip.AnchorToRect(this.transform as RectTransform);

        // Show the tooltip
        UITooltip.Show();
    }
    else
    {
        // Hide the tooltip
        UITooltip.Hide();
    }
}
```

## Complete MyCustomSlot Script

```
using UnityEngine;
using UnityEngine.Events;
using UnityEngine.EventSystems;
using System;
using Object = UnityEngine.Object;

namespace DuloGames.UI
{
    public class MyCustomSlot : UISlotBase
    {
        public class MyCustomInfo
        {
            public int id;
            public Sprite icon;
            public string name;
        }

        [Serializable] public class OnClickEvent : UnityEvent<MyCustomSlot> { }
        [Serializable] public class OnAssignEvent : UnityEvent<MyCustomSlot> { }
        [Serializable] public class OnUnassignEvent : UnityEvent<MyCustomSlot> { }

        /// <summary>
        /// The assigned custom info.
        /// </summary>
        private MyCustomInfo m_MyCustomInfo;

        /// <summary>
        /// The click event delegate.
        /// </summary>
        public OnClickEvent onClick = new OnClickEvent();

        /// <summary>
        /// The assign event delegate.
        /// </summary>
        public OnAssignEvent onAssign = new OnAssignEvent();

        /// <summary>
        /// The unassign event delegate.
        /// </summary>
        public OnUnassignEvent onUnassign = new OnUnassignEvent();

        /// <summary>
        /// Gets the info assigned to this slot.
        /// </summary>
        /// <returns>The info.</returns>
        public MyCustomInfo GetInfo()
        {
            return this.m_MyCustomInfo;
        }

        /// <summary>
        /// Determines whether this slot is assigned.
        /// </summary>
        /// <returns><c>true</c> if this instance is assigned; otherwise, <c>false</c>.</returns>
        public override bool IsAssigned()
        {
            return (this.m_MyCustomInfo != null);
        }

        /// <summary>
        /// Assign the slot by info.
        /// </summary>
        /// <param name="info">The info.</param>
        public bool Assign(MyCustomInfo info)
        {
            if (info == null)
                return false;

            // Make sure we unassign first, so the event is called before new assignment
            this.Unassign();

            // Use the base class assign to set the icon
            this.Assign(info.icon);

            // Set the info
            this.m_MyCustomInfo = info;

            // Invoke the on assign event
            if (this.onAssign != null)
                this.onAssign.Invoke(this);

            // Success
            return true;
        }

        /// <summary>
        /// Assign the slot by the passed source slot.
        /// </summary>
        /// <param name="source">Source.</param>
        public override bool Assign(Object source)
        {
            if (source is MyCustomSlot)
            {
                MyCustomSlot sourceSlot = source as MyCustomSlot;

                if (sourceSlot != null)
                    return this.Assign(sourceSlot.GetInfo());
            }

            // Default
            return false;
        }

        /// <summary>
        /// Unassign this slot.
        /// </summary>
        public override void Unassign()
        {
            // Remove the icon
            base.Unassign();

            // Clear the info
            this.m_MyCustomInfo = null;

            // Invoke the on unassign event
            if (this.onUnassign != null)
                this.onUnassign.Invoke(this);
        }

        /// <summary>
        /// Determines whether this slot can swap with the specified target slot.
        /// </summary>
        /// <returns><c>true</c> if this instance can swap with the specified target; otherwise, <c>false</c>.</returns>
        /// <param name="target">Target.</param>
        public override bool CanSwapWith(Object target)
        {
            // We can only swap with another MyCustomSlot
            return (target is MyCustomSlot);
        }

        /// <summary>
        /// Performs a slot swap.
        /// </summary>
        /// <returns><c>true</c>, if slot swap was performed, <c>false</c> otherwise.</returns>
        /// <param name="targetObject">The other slot.</param>
        public override bool PerformSlotSwap(Object targetObject)
        {
            // Get the other slot
            MyCustomSlot targetSlot = (targetObject as MyCustomSlot);

            // Get the other slot info
            MyCustomInfo targetInfo = targetSlot.GetInfo();

            // Assign the other slot by this slot
            bool assign1 = targetSlot.Assign(this.GetInfo());

            // Assign this slot by the other slot info
            bool assign2 = this.Assign(targetInfo);

            // Return the status
            return (assign1 && assign2);
        }

        /// <summary>
        /// Raises the tooltip event.
        /// </summary>
        /// <param name="show">If set to <c>true</c> show.</param>
        public override void OnTooltip(bool show)
        {
            // Make sure we have info
            if (this.m_MyCustomInfo == null)
                return;

            // If we are showing the tooltip
            if (show)
            {
                UITooltip.InstantiateIfNecessary(this.gameObject);

                // Set the title
                UITooltip.AddTitle(this.m_MyCustomInfo.name);

                // Anchor to this slot
                UITooltip.AnchorToRect(this.transform as RectTransform);

                // Show the tooltip
                UITooltip.Show();
            }
            else
            {
                // Hide the tooltip
                UITooltip.Hide();
            }
        }

        /// <summary>
        /// Raises the pointer click event.
        /// </summary>
        /// <param name="eventData">Event data.</param>
        public override void OnPointerClick(PointerEventData eventData)
        {
            base.OnPointerClick(eventData);

            // Make sure the slot is assigned
            if (!this.IsAssigned())
                return;

            // Check for left click
            if (eventData.button == PointerEventData.InputButton.Left)
            {
                // Invoke the click event
                if (this.onClick != null)
                    this.onClick.Invoke(this);
            }
        }
    }
}
```

Note: the slot editor only draws the Slot Base properties. Public UnityEvent fields of your custom slot will not show in the inspector unless you write a custom editor (see `UISpellSlotEditor` for an example). You can still add listeners from code.
