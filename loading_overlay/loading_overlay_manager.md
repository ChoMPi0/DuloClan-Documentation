# Loading Overlay Manager
This is a scriptable object which handles the instantiation of [Loading Overlay](loading_overlay.md).

## Simple Usage
The manager must be placed in a Resources directory in your project.

To create the manager right click in your Project view then select Create -> UI Managers -> Loading Overlay Manager.

Assign the [Loading Overlay](loading_overlay.md) prefab.

## Code Usage
You can access the loading overlay manager at any time by the Instance static property.
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
```
