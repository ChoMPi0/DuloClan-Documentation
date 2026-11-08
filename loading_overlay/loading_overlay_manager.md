# Loading Overlay Manager
A ScriptableObject that holds the [Loading Overlay](loading_overlay.md) prefab and creates overlays for scene loading.

## Setup
1. Right click in the Project view and select Create -> UI Managers -> Loading Overlay Manager.
2. Save the asset in a **Resources** folder and keep the default name **LoadingOverlayManager**. It is loaded with `Resources.Load("LoadingOverlayManager")`, so a renamed asset or one outside a Resources folder is not found.
3. Assign the [Loading Overlay](loading_overlay.md) prefab to **Loading Overlay Prefab**.

## Inspector Settings

| Property | Summary |
| --- | --- |
| Loading Overlay Prefab | The prefab instantiated by `Create`. It must have a UILoadingOverlay component. |

## Methods

| Name | Returns | Summary |
| --- | --- | --- |
| Create() | UILoadingOverlay | Instantiates the prefab at the scene root (the overlay has its own canvas). Returns null when no prefab is assigned. |

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| prefab | GameObject | The loading overlay prefab. |

## Static Properties

| Name | Type | Summary |
| --- | --- | --- |
| Instance | UILoadingOverlayManager | The manager asset loaded from Resources, or null. |

## Code Usage
To load a scene from a button without code, use [Load Scene](load_scene.md).

```
public void LoadScene(string sceneName)
{
    if (UILoadingOverlayManager.Instance != null)
    {
        UILoadingOverlay loadingOverlay = UILoadingOverlayManager.Instance.Create();

        if (loadingOverlay != null)
        {
            loadingOverlay.LoadScene(sceneName);
        }
        else
        {
            Debug.LogWarning("Failed to instantiate the loading overlay prefab, make sure it's assigned on the manager.");
        }
    }
}

public void LoadSceneByIndex(int buildIndex)
{
    if (UILoadingOverlayManager.Instance == null)
        return;

    UILoadingOverlay loadingOverlay = UILoadingOverlayManager.Instance.Create();

    if (loadingOverlay != null)
        loadingOverlay.LoadScene(buildIndex);
}
```
