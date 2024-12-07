# Loading Overlay
A simple overlay to visualize loading of scenes with a progress bar.

## Simple Usage
Attach the component to a GameObject outside of a canvas and assign the required fields.

After configuring your loading overlay, create a prefab of it and assign it to the [Loading Overlay Manager](loading_overlay_manager.md).

The visibility of the overlay is controlled by a CanvasGroup alpha value.

To load a scene from a button you could use the [Load Scene component](load_scene.md) or via code.

## Code Usage
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
