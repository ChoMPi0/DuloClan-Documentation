# Scene Registry
The scene registry keeps track of every [UI Scene](scene.md). Scenes register themselves; you don't need to add anything to the scene. Access it through the `UISceneRegistry.instance` static property.

## Methods

| Name | Returns | Summary |
| --- | --- | --- |
| GetScene(int) | UIScene | Gets the scene with the given ID. |
| GetActiveScenes() | UIScene[] | Gets the scenes that are currently shown. |
| GetAvailableSceneId() | int | Gets an unused scene ID. |
| TransitionToScene(UIScene) | void | Hides the active scenes and shows the given one, each with its own transition. |

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| scenes | UIScene[] | All registered scenes. |
| lastScene | UIScene | The previously shown scene (used by "back" buttons). |

## Code Usage
```
public void GoBack()
{
    UIScene last = UISceneRegistry.instance.lastScene;

    if (last != null)
        UISceneRegistry.instance.TransitionToScene(last);
}
```

The older `UISceneManager` component is superseded by the registry and is no longer needed.
