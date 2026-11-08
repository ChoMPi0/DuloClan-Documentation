# Canvas Helpers
Three components that keep a canvas lined up with a camera. All of them update every frame, also in edit mode.

## Canvas Anchor To Camera
Keeps a RectTransform at a viewport point of a camera.

1. Add **UI Canvas Anchor To Camera** to an object with a RectTransform.
2. Assign the **Camera**.
3. Set **Horizontal** and **Vertical** (0 to 1). For example 0 / 0 is the bottom left of the view and 0.5 / 0.5 is the center.

The point is taken at the camera's far clip plane. Only X and Y are changed; the object keeps its own Z position.

## Canvas Aspect Resize
Resizes a RectTransform's width so it matches a camera's aspect ratio.

1. Add **UI Canvas Aspect Resize** to an object with a RectTransform.
2. Assign the **Camera**.

The width is set to the current height multiplied by the camera aspect. The height is not changed.

## World Canvas Scale By Camera
Scales a world space canvas so its height fills the camera view.

1. Add **UI World Canvas Scale By Camera** to any object.
2. Assign the **Camera** and the **Canvas**.

For a perspective camera the visible height is calculated at the distance between the camera and the canvas. For an orthographic camera it is the orthographic size times two. The canvas local scale X and Y are set so the canvas rect height matches that visible height; Z scale is set to 1.
