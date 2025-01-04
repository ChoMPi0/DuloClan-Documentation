# Slot Base
This component has the base functionality for the icon slots [Item Slot](item_slot.md), [Spell Slot](spell_slot.md) and [Equip Slot](equip_slot.md).

## How To Extend
Let's start building a new slot by extending from the **Slot Base** class.

```
using UnityEngine;
using UnityEngine.Events;
using UnityEngine.EventSystems;
using System;
using DuloGames.UI;
using Object = UnityEngine.Object;

public class CustomSlot : UISlotBase { }
```

Now define a class to contain our custom slot info.

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
/// <returns>The spell info.</returns>
public MyCustomInfo GetInfo()
{
    return this.m_MyCustomInfo;
}
```

To keep track of what the slots contain we should add on assign and on unassign events so we can save that info. Also the use the slot we would need a click event.

Here is how to define the events we are going to use.

```
[Serializable] public class OnClickEvent : UnityEvent<CustomSlot> { }
[Serializable] public class OnAssignEvent : UnityEvent<CustomSlot> { }
[Serializable] public class OnUnassignEvent : UnityEvent<CustomSlot> { }

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

In order to check if the slot is assigned we need to override the **IsAssigned()** method.

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

In order to handle the assign process of our new custom slot we must override the **Assign(Object source)** method. We are also going to define a new method to help us assign the slot by third party scripts.

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

After we have handled the assign process let's do the unassign.

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

We're almost done, we still need to override the swapping methods to handle swapping of our custom slot.

```
/// <summary>
/// Determines whether this slot can swap with the specified target slot.
/// </summary>
/// <returns><c>true</c> if this instance can swap with the specified target; otherwise, <c>false</c>.</returns>
/// <param name="target">Target.</param>
public override bool CanSwapWith(Object target)
{
    if (target is MyCustomSlot)
    {
        // It's MyCustomSlot we can swap
        return true;
    }

    // Default
    return false;
}

// <summary>
/// Performs a slot swap.
/// </summary>
/// <returns><c>true</c>, if slot swap was performed, <c>false</c> otherwise.</returns>
/// <param name="sourceSlot">Source slot.</param>
public override bool PerformSlotSwap(Object sourceObject)
{
    // Get the source slot
    MyCustomSlot sourceSlot = (sourceObject as MyCustomSlot);

    // Get the source item info
    MyCustomInfo sourceInfo = sourceSlot.GetInfo();

    // Assign the source slot by this slot
    bool assign1 = sourceSlot.Assign(this.GetInfo());

    // Assign this slot by the source slot
    bool assign2 = this.Assign(sourceInfo);

    // Return the status
    return (assign1 && assign2);
}
```

Now to handle the click event we must override the **OnPointerClick(PointerEventData eventData)** method.

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

Finally here's how to handle tooltips.

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

        // Prepare the tooltip
        // Set the title and description
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
        [Serializable]  public class OnUnassignEvent : UnityEvent<MyCustomSlot> { }
        
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
        /// <returns>The spell info.</returns>
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
            if (target is MyCustomSlot)
            {
                // It's MyCustomSlot we can swap
                return true;
            }

            // Default
            return false;
        }

        // <summary>
        /// Performs a slot swap.
        /// </summary>
        /// <returns><c>true</c>, if slot swap was performed, <c>false</c> otherwise.</returns>
        /// <param name="sourceSlot">Source slot.</param>
        public override bool PerformSlotSwap(Object sourceObject)
        {
            // Get the source slot
            MyCustomSlot sourceSlot = (sourceObject as MyCustomSlot);

            // Get the source item info
            MyCustomInfo sourceInfo = sourceSlot.GetInfo();

            // Assign the source slot by this slot
            bool assign1 = sourceSlot.Assign(this.GetInfo());

            // Assign this slot by the source slot
            bool assign2 = this.Assign(sourceInfo);

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

                // Prepare the tooltip
                // Set the title and description
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
