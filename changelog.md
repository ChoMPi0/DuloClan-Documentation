# Script Changelog
Changes to the shared UI scripts. Your asset's version is in its `Readme.txt`; asset-specific changes are in its `Changelog.txt`.

## 1.3 (November 2026)
Requires Unity 2022.3 LTS or newer.

**Controls**
- Select field: fixed the option font (the built-in Arial font was removed from Unity; now uses LegacyRuntime), also for select fields added at runtime.
- Select field: the list and its click blocker no longer stay on screen when the field is disabled while open.
- Select field: keyboard and gamepad navigation now selects options; **Cancel** closes the list.
- Select field: no errors when selecting out-of-range options or with nothing selected.
- Slider extended: no longer creates or destroys objects inside OnValidate; fixed errors with one or no options.
- Tab: active tabs keep their active look after being clicked; fixed an error with no sprite.

**Tooltips and slots**
- Tooltip: no longer lost when it is recreated for another canvas; no duplicated lines when shown during a fade out.
- Cooldowns: no longer leak between play sessions; restarting a cooldown replaces the old one.
- Equip receiver: items without a matching equip slot are no longer destroyed when dropped on it.

**Windows, modal boxes and scenes**
- Window: the black overlay is released when a window is disabled while open; no duplicate overlay listeners.
- Modal box: only the top-most box reacts to Submit/Cancel; hidden or closing boxes ignore input; events fire once.
- Loading overlay: loading by scene name works for scenes that aren't loaded yet (resolved through the build settings).
- UI Scene: no error when deactivating a prefab scene that was never activated; no duplicate instances on re-activation.

**Other**
- Tweens: finish callbacks always run, even when the object is inactive (windows, modal boxes and select fields always clean up).
- Color schemes: apply to inactive objects too; changes support Undo and are saved as prefab overrides.
- Navigation tools: no longer modify prefab assets on disk; changes support Undo.
- Mask raycast filter: uses Unity's built-in alpha hit test (supports sliced, tiled, filled and atlas sprites) and no longer changes texture import settings.
- Letter spacing: correct with spaces, line breaks and rich text tags.
- Enter Play Mode Options: all managers and static state work with domain reload disabled.

## 1.2
- Fixed deprecation warnings for Unity 6.
