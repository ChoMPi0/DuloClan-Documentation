# Troubleshooting

## Input errors

**`ArgumentException: Input Button I is not setup.`**
A window or component uses a named input that doesn't exist in your project. Add it in **Project Settings -> Input Manager** (see [Input Setup](getting_started.md#input-setup)), or clear the input name on the component.

**`InvalidOperationException: You are trying to read Input using the UnityEngine.Input class, but you have switched active Input handling to Input System package`**
The scripts use the legacy Input Manager. Set **Project Settings -> Player -> Active Input Handling** to **Both**.

## Tooltips

**Tooltips never appear**
- Check that the **TooltipManager** asset is in a Resources folder and has the **Tooltip** prefab assigned ([Tooltip Manager](../tooltips/tooltip_manager.md)).
- The scene needs an **EventSystem**, and the hovered object needs a graphic with **Raycast Target** on.
- On slots, check that **Enabled** is on in the slot's Tooltip section. The tooltip appears after **Display Delay**.

**The tooltip is behind other UI or clipped**
The tooltip is created on the canvas of the hovered object and kept on top of that canvas by [Always On Top](../miscellaneous/always_on_top.md). If another canvas has a higher **Sort Order**, or the hovered object is inside a nested canvas with a mask, the tooltip can render below it or be clipped. Keep hoverable elements on the main canvas where possible.

## Windows and modal boxes

**A window doesn't open with its key**
Keys that toggle windows are named inputs (for example through [Input Event](../miscellaneous/input_event.md)); add them to the Input Manager. The demo uses I, C, P and L. Input Event ignores keys while another input field or control is selected.

**Escape closes nothing**
The [Window Manager](../windows/window_manager.md) listens to the **Cancel** input. Make sure it exists and the window's **Escape Key Action** is not None.

**Modal boxes don't show**
The **ModalBoxManager** asset must be in a Resources folder with the Modal Box prefab assigned ([Modal Box Manager](../modal_box/modal_box_manager.md)).

## Scenes and loading

**Load Scene / Loading Overlay does nothing**
The scene must be in **File -> Build Profiles** (Build Settings). An error names the missing scene. The **LoadingOverlayManager** asset needs its prefab assigned ([Loading Overlay Manager](../loading_overlay/loading_overlay_manager.md)).

## Visuals

**The UI is too big or too small**
The prefabs are made for a **Canvas Scaler** with *Scale With Screen Size* and a reference resolution of **3840x2160**.

**A color scheme doesn't change some objects**
Only objects with a **Color Scheme Element** are recolored. Add one and choose the shade, then apply the scheme again ([Color Schemes](../color_schemes/color_schemes.md)).

**The Mask Raycast Filter has no effect**
Enable **Read/Write** on the sprite's texture. A warning in the console names the texture ([Mask Raycast Filter](../raycast_filters/mask_raycast_filter.md)).

**`'Gradient' is an ambiguous reference between 'DuloGames.UI.Gradient' and 'UnityEngine.Gradient'`**
Write the full name `DuloGames.UI.Gradient` ([Gradient](../effects/gradient.md)).

## Still stuck?
Check the [Script Changelog](../changelog.md) to see if the issue was fixed in a newer version, then contact us with your Unity version, the asset version (Readme.txt) and the full console error.
