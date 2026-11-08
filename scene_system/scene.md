# UI Scene
A UI Scene is a full screen page of the interface (login, character select, options, ...). Scenes are registered in the [Scene Registry](scene_registry.md) and switched with transitions.

## Setup
1. Create a full screen object under your canvas and add **UI Scene** (a CanvasGroup is added automatically).
2. Give it a unique **ID**.
3. Choose the **Type** and assign its content.
4. Choose the **Transition** used when the scene is shown or hidden.
5. Use [Scene Open](scene_open.md) on buttons to switch scenes.

## Types

| Type | Content | Behavior |
| --- | --- | --- |
| Preloaded | **Content** transform already in the scene | Shown and hidden; never destroyed. |
| Prefab | **Prefab** reference | Instantiated on activate, destroyed on deactivate. |
| Resource | **Resource** path in a Resources folder | Loaded and instantiated on activate, destroyed on deactivate (unused assets are unloaded). |

## Transitions
None, Animation, CrossFade, SlideFromRight, SlideFromLeft, SlideFromTop, SlideFromBottom.

**Transition Duration** and **Transition Easing** control the tween. The **Animation** transition sets the **Animate In Trigger** / **Animate Out Trigger** on the scene's Animator.

**First Selected** is selected in the EventSystem when the scene is shown (keyboard and gamepad navigation).

## Events
`onActivate` and `onDeactivate` pass the scene.

```
public void OnSceneActivate(UIScene scene)
{
    Debug.Log("Scene " + scene.id + " shown");
}
```

## Object Oriented Methods

| Name | Returns | Summary |
| --- | --- | --- |
| TransitionTo() | void | Transitions to this scene through the registry (hides the active scene). |
| TransitionIn() | void | Shows the scene with its transition. |
| TransitionIn(Transition, float, TweenEasing) | void | Shows the scene with a custom transition. |
| TransitionOut() | void | Hides the scene with its transition. |
| TransitionOut(Transition, float, TweenEasing) | void | Hides the scene with a custom transition. |
| Activate() | void | Shows the scene instantly. |
| Deactivate() | void | Hides the scene instantly. |
| SetCanvasAlpha(float) | void | Sets the canvas group alpha. |

## Properties

| Name | Type | Summary |
| --- | --- | --- |
| id | int | The scene ID. |
| isActivated | bool | Is the scene shown. Setting it activates or deactivates. |
| type | UIScene.Type | The scene type. |
| content | Transform | The scene content. |
| transition | UIScene.Transition | The transition type. |
| transitionDuration | float | The transition duration in seconds. |

## Code Usage
```
public void OpenOptions()
{
    UIScene scene = UISceneRegistry.instance.GetScene(3);

    if (scene != null)
        scene.TransitionTo();
}
```
