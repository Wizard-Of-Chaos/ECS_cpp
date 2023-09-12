# ECS_cpp
An implementation of an Entity-Component-System in simple C++.

ECS logic initially described here: https://www.david-colson.com/2020/02/09/making-a-simple-ecs.html

This was the initial ECS used for Extermination Shock until we switched to flecs.

Use:
Create some components defining what exactly you want in your game. For example, a health component.
```cpp
struct Health
{
  float currentHealth = 0;
  float maxHealth = 0;
};
```

Now, operate on these entities through systems using the SceneView class.

```cpp
void healthSystem() 
{
  for (auto id : SceneView<HealthComponent>(scene)) {
    Health* hp = scene.get<HealthComponent>(id);
    if(hp == 0) killEntity(id);
  }
}
```

In order to use these systems, we have to call an update function using the SceneManager class. We can define this ourselves.

The update function must be a `void` function with arguments of `float, float, Scene&` -- for delta time, frame delta, and the scene respectively.

```cpp
void myCustomUpdate(float dt, float frameDelta, Scene& scene) 
{
  healthSystem();
}
```

Initialize the scene and the scene manager to define objects in said scene and register your custom update function with the scene manager.

```cpp
SceneManager sceneManager;
sceneManager.setUpdateFunction(myCustomUpdate);
```

Then, in your game loop, call `sceneManager.update(dt, frameDelta)` to update all your systems accordingly for that tick.
