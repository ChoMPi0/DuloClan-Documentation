# Load Scene
This component is used to simplify loading of scenes, it could also use the [Loading Overlay](loading_overlay.md).

## Simple Usage
Attach the component to any GameObject and assign the required fields.

Enter the scene name or build index into the **Scene** field. The scene must be in the build settings.

Check the **Use Loading Overlay** toggle if you want to use it.

Select an **Input Key** or assign a Button to the **Hook To Button** field. The input is ignored while a [Modal Box](../modal_box/modal_box.md) is open.

## Code Usage
You can start loading the scene via code by invoking the **LoadScene()** method.
