# Getting Started

## Requirements
- Unity 6 or newer.
- The **Unity UI** package (`com.unity.ugui`), installed by default. It includes TextMeshPro.
- The **TMP Essential Resources**: **Window -> TextMeshPro -> Import TMP Essential Resources** (once per project). See [TextMeshPro](textmeshpro.md).
- The scripts use the legacy **Input Manager** (Project Settings -> Player -> Active Input Handling: *Input Manager (Old)* or *Both*).

## Manager Assets
Several systems are configured by a ScriptableObject that is loaded by name from a **Resources** folder. The assets ship in the asset's `Resources` folder; keep them there (or in any other `Resources` folder) and keep the names unchanged.

| Asset | Used by | Page |
| --- | --- | --- |
| TooltipManager | Tooltips, slot tooltips | [Tooltip Manager](../tooltips/tooltip_manager.md) |
| ModalBoxManager | Modal boxes | [Modal Box Manager](../modal_box/modal_box_manager.md) |
| LoadingOverlayManager | Loading overlay, Load Scene | [Loading Overlay Manager](../loading_overlay/loading_overlay_manager.md) |
| BlackOverlayManager | Window black overlay | [Black Overlay Manager](../windows/black_overlay_manager.md) |
| ColorSchemeManager | Color schemes | [Color Schemes](../color_schemes/color_schemes.md) |
| Databases/ItemDatabase, SpellDatabase, TalentDatabase | Demo slots | [Databases](../icon_slots/databases.md) |

To create a new manager, right click in the Project view and select **Create -> UI Managers -> ...**.

## Scene Setup
Every UI scene needs:
1. A **Canvas** with a **Graphic Raycaster**.
2. An **EventSystem** (GameObject -> UI -> Event System).
3. A **Window Manager** if you use [Windows](../windows/window.md) (it is created automatically when the first window starts).
4. An [Audio Source](../audio/audio_source.md) if you use [Play Audio](../audio/play_audio.md).

## Input Setup
Some windows and components use named inputs. Add them in **Project Settings -> Input Manager** if they don't exist:

| Input name | Used for |
| --- | --- |
| Cancel | Closing windows and modal boxes (Escape), closing select field lists |
| Submit | Confirming modal boxes |
| I | Inventory window (demo) |
| C | Character window (demo) |
| P | Spell book window (demo) |
| L | Quest log window (demo) |

## Canvas Scaling
The demo scenes use a **Canvas Scaler** set to *Scale With Screen Size* with a reference resolution of **3840x2160** (4K). The images are sliced from the PSD files scaled up to 200%. Use the same reference resolution in your own canvases so the prefabs keep their intended size.

## Enter Play Mode Options
All scripts support **Enter Play Mode Options** with domain reload disabled; static state is reset when entering play mode.

## Next Steps
- Build a window: [Window](../windows/window.md).
- Add tooltips: [Tooltip](../tooltips/tooltip.md) and [Tooltip Show](../tooltips/tooltip_show.md).
- Inventory and action bars: [Slot Base](../icon_slots/slot_base.md).
- Recolor the whole UI: [Color Schemes](../color_schemes/color_schemes.md).
